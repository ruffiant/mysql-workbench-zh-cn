# mysql-workbench-zh-cn
mysql-workbench 汉化
我是用python,跑的 D:\Program Files\MySQL\MySQL Workbench 8.0 CE\data
用google 翻译的文件
其中包括
```
dbquery_toolbar.xml
db_datatype_groups.xml
default_toolbar.xml
main_menu.xml
model_diagram_toolbar.xml
model_option_toolbar_layer.xml
model_option_toolbar_note.xml
model_option_toolbar_physical_relationship.xml
model_option_toolbar_physical_routinegroup.xml
model_option_toolbar_physical_table.xml
model_option_toolbar_physical_view.xml
model_option_toolbar_selection.xml
model_toolbar.xml
paper_types.xml
shortcuts.xml
shortcuts_basic.xml
shortcuts_physical.xml
table_templates.xml
tools_toolbar.xml
tools_toolbar_basic.xml
tools_toolbar_physical.xml
```
```python
# -*- coding: utf-8 -*-
"""
Created on Fri Aug 23 10:25:51 2024

@author: WIN
"""
import os
import re
from googletrans import Translator

# 初始化翻译器
translator = Translator()

# 定义字典，指定特定单词的翻译
translation_dict = {
    'open': '打开',
    'separator': '分隔符',
    'home': '主页',
    'find': '查找',
    'model': '模型',
    'view': '视图',
    'scripting': '脚本',
    'migration wizard': '迁移向导',
    'new': '新建',
    # 可以添加更多单词和翻译
}

# XML文件路径
xml_file_path = './mysqldata/en/'
# 输出文件路径
output_dir = './mysqldata/zh/'
# 正则表达式模式
pattern = r'(<value type="(.*?)" key="(.*?)">)(.*?)(</value>)'
# 需要翻译的键
key_arr = ['caption', 'description', 'tooltip', 'accessibilityName']

# 确保输出目录存在
os.makedirs(output_dir, exist_ok=True)

# 遍历输入目录中的所有 XML 文件
for filename in os.listdir(xml_file_path):
    if filename.endswith('.xml'):
        input_file = os.path.join(xml_file_path, filename)
        output_file = os.path.join(output_dir, filename)

        with open(input_file, 'r', encoding='utf-8') as infile, open(output_file, 'w', encoding='utf-8') as outfile:
            for i, line in enumerate(infile):
                print(f'\rProcessing file: {filename}, line {i + 1}', end='')
                # 查找符合模式的行
                match = re.search(pattern, line)

                if match:
                    key_str = match.group(3)
                    if key_str in key_arr:
                        line_str = match.group(4)  # 获取value标签内的文本
                        # 使用翻译器翻译整个文本
                        try:
                            translation = translator.translate(line_str, src='en', dest='zh-cn')
                            translated_text = translation.text

                            # 使用字典替换特定单词
                            for key, value in translation_dict.items():
                                translated_text = translated_text.replace(key, value)

                            # 替换原始文本为翻译后的文本
                            line = re.sub(pattern, f'{match.group(1)}{translated_text}{match.group(5)}', line)
                        except Exception as e:
                            print(f"Error translating: {e}")  # 打印错误信息

                outfile.write(line)  # 写入输出文件
```

当然我翻译的并不是很精准,如果你翻译了更好的,希望你能提交上来