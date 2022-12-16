import xlrd
import xlwt


def read_excel():
    workbook = xlrd.open_workbook('D:/1155178107/GIS5011_GIS/project/Spider/Spider/旺角.xls') #读取源excel文件，最好利用绝对路径（完整路径，从哪个盘开始）
    jieguo = xlwt.Workbook(encoding="ascii")  #生成excel
    wsheet = jieguo.add_sheet('sheet name') #生成sheet
    sheetnum=workbook.nsheets  #获取源文件sheet数目
    y=0 #生成的excel的行计数
    for m in range(0,sheetnum):
        sheet = workbook.sheet_by_index(m) #读取源excel文件第m个sheet的内容
        nrowsnum=sheet.nrows  #获取该sheet的行数
        for i in range(0,nrowsnum):
            date=sheet.row(i) #获取该sheet第i行的内容
            for n in range(0,len(date)):
                aaa=str(date[n]) #把该行第n个单元格转化为字符串，目的是下一步的关键字比对
                if aaa.find('道')>0: #进行关键字比对，包含关键字返回1，否则返回0
                    y=y+1
                    for j in range(len(date)):
                        wsheet.write(y,j,sheet.cell_value(i,j)) #该行包含关键字，则把它所有单元格依次写入入新生成的excel的第y行

    jieguo.save('道.xls') #保存新生成的Excel



if __name__ == '__main__':
    read_excel()
