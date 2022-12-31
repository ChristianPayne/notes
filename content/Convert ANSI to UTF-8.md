---
title: Convert ANSI to UTF-8
---
This script was created to solve the problem of AWS Glue jobs failing because our files in S3 were stored as ANSI format. Glue apparently only allows UTF-8 encoding on the files it processes.

The difference between the Mac and Windows scripts are the file path method and the encoding naming. They will do the same task, just configured for each OS.

## Prerequisites
To run this on your machine, you will need to have python 3.x installed. No other requirements are necessary.

## Usage
The easiest way to run this is to put the python script next to the folder that you want to run it on. 
1. Save the script as `ANSI_to_UTF8.py`
2. Navigate to that directory and run:

```
python ANSI_to_UTF8.py <folder name>
```

The script will copy all the files into a new directory with UTF_8 encoding.

## Mac
```python
import sys
import os

if len(sys.argv) != 2:
  print(f"Converts the contents of a folder to UTF-8 from ASCI.")
  print(f"USAGE: \n\
    python ANSI_to_UTF8.py <Relative_Folder_Name> \n\
    If targeting a nested folder, make sure to use an escaped \\. ie: parent\\\\child")
  sys.exit()

from_encoding = "cp1252"
to_encoding = "UTF-8"
list_of_files = []
current_dir = os.getcwd()
folder = sys.argv[1]
suffix = "_utf8"
target_folder = folder + "_utf8"


try:
  os.mkdir(target_folder)
except FileExistsError:
  print("Target folder already exists.")
except:
  print("Error making directory!")

for root, dirs, files in os.walk(folder):
	for file in files:
		list_of_files.append(os.path.join(root,file))


for file in list_of_files:
  print(f"Converting {file}")

  original_path = file

  filename = file.split("/")[-1].split(".")[0]
  extension = file.split("/")[-1].split(".")[1]
  folder = "/".join(original_path.split("/")[0:-1])
  new_filename = filename + "." + extension
  new_path = os.path.join(target_folder, new_filename)

  f= open(original_path, 'r', encoding=from_encoding)
  content= f.read()
  f.close()
  f= open(new_path, 'w', encoding=to_encoding)
  f.write(content)
  f.close()

print(f"Finished converting {len(list_of_files)} files to {target_folder}")
```

## Windows
```python
import sys
import os

if len(sys.argv) != 2:
  print(f"Converts the contents of a folder to UTF-8 from ASCI.")
  print(f"USAGE: \n\
    python ANSI_to_UTF8.py <Relative_Folder_Name> \n\
    If targeting a nested folder, make sure to use an escaped \\. ie: parent\\\\child")
  sys.exit()

from_encoding = "ANSI"
to_encoding = "UTF-8"
list_of_files = []
current_dir = os.getcwd()
folder = sys.argv[1]
suffix = "_utf8"
target_folder = folder + "_utf8"


try:
  os.mkdir(target_folder)
except FileExistsError:
  print("Target folder already exists.")
except:
  print("Error making directory!")

for root, dirs, files in os.walk(folder):
    for file in files:
        list_of_files.append(os.path.join(root,file))


for file in list_of_files:
  print(f"Converting {file}")

  original_path = file

  filename = file.split("\\")[-1].split(".")[0]
  extension = file.split("\\")[-1].split(".")[1]
  folder = "\\".join(original_path.split("\\")[0:-1])
  new_filename = filename + "." + extension
  new_path = os.path.join(target_folder, new_filename)

  f= open(original_path, 'r', encoding=from_encoding)
  content= f.read()
  f.close()
  f= open(new_path, 'w', encoding=to_encoding)
  f.write(content)
  f.close()

print(f"Finished converting {len(list_of_files)} files to {target_folder}")
```