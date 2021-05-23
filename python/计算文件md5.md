# 计算文件md5值





```
import hashlib
import os
 
def get_md5(file_path):
  md5 = None
  if os.path.isfile(file_path):
    f = open(file_path,'rb')
    md5_obj = hashlib.md5()
    md5_obj.update(f.read())
    hash_code = md5_obj.hexdigest()
    f.close()
    md5 = str(hash_code).lower()
  return md5
 
if __name__ == "__main__":
  file_path = '/Users/wengmingqiang/wengmq/test/file_records.json'
  md5_01 = get_md5(file_path)
  print(md5_01)
```



