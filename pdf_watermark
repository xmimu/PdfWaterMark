from pathlib import Path
import sys
import PyPDF2

OUT = Path()
WATERMARK_FILE = Path()


def add_watermark(water_file, page_pdf):
    pdfReader = PyPDF2.PdfFileReader(water_file)
    page_pdf.mergePage(pdfReader.getPage(0))
    return page_pdf


def add_watermark_to_file(path: Path):
    pdfWriter = PyPDF2.PdfFileWriter()  # 用于写pdf
    pdfReader = PyPDF2.PdfFileReader(path.open(mode='rb'))  # 读取pdf内容
    # 遍历pdf的每一页,添加水印
    for page in range(pdfReader.numPages):
        page_pdf = add_watermark(WATERMARK_FILE.open(mode='rb'), pdfReader.getPage(page))
        pdfWriter.addPage(page_pdf)

    marked_file = OUT / ('marked-' + path.name)
    pdfWriter.write(marked_file.open(mode='wb'))
    print(marked_file)


def check_path():
    global OUT, WATERMARK_FILE
    if not OUT.exists():
        OUT.mkdir()
    if not WATERMARK_FILE.exists():
        print('先创建一个水印文件，用word做水印保存成"watermark.pdf"')
        return False
    return True


def is_pdf(path):
    return path.exists() and path.suffix == '.pdf' and path.stem != 'watermark'


def main(argv):
    global OUT, WATERMARK_FILE
    script = Path(argv[0]).absolute()
    OUT = script.parent / 'marked'
    WATERMARK_FILE = script.parent / 'watermark.pdf'
    # 检查文件目录
    if not check_path(): return
    # 遍历参数列表
    for i in argv:
        path = Path(i).absolute()
        if not is_pdf(path): continue
        add_watermark_to_file(path)


if __name__ == '__main__':
    try:
        main(sys.argv)
    except Exception as e:
        print(e)
    input('任意键退出...')
