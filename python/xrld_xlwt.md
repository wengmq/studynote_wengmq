# xlwt xlrd

xlrd 官方API参考：https://xlrd.readthedocs.io/en/latest/api.html

xlwt官方说明：https://xlwt.readthedocs.io/en/latest/api.html

demo:

```
import xlwt
from xlrd import open_workbook

#读取的xls文件
old_file = open_workbook('IdoWorkList_20201015210400.xls')
old_sheet = old_file.sheet_by_index(-1)

#要保存到的新的xls文件
new_file = xlwt.Workbook(encoding='utf-8', style_compression = 0)
new_sheet = new_file.add_sheet('Sheet1', cell_overwrite_ok = True)

#获取工作表行数和列数
rows = old_sheet.nrows 
cols = old_sheet.ncols 

#找出域名列表中域名数量小于6的具体行数,存入need_rows
need_rows = list()
expire_num = 6
for row in range(rows):
    domain_list = str(old_sheet.cell(row,12).value)
    num = len(domain_list.splitlines())
    if num < expire_num:
            need_rows.append(row)

#读取need_rows中的row信息，将我们需要的行write新xls文件中
write_row = 0
for row in need_rows:
    if(write_row >= len(need_rows)):
        break
    for col in range(cols):
        cell = old_sheet.cell(row,col).value
        new_sheet.write(write_row, col, cell)
    write_row = write_row + 1

#保存到电脑
new_file.save('result.xls')
```