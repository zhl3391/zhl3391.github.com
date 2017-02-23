title: 我在胡说八道:)
date: 2017-02-22 10:41:12
---
kim正在胡说八道...

### 2017-02-23

终于写好了生成模版代码的脚本，以后写类似的需求效率就高多了。哈哈。
根据自身项目编写，仅供参考。

```python
#coding=utf-8

import json
import codecs
import time

def create(templateName, type, config, root=''):
	template = ''
	with codecs.open(templateName, 'rb', 'UTF-8') as f:
		template = f.read()
	if not template:
		return
	template = template % config;
	dir = config['src_dir'] + config['module'] + root
	filename = dir + config['class_name'] + type + '.java'
	with codecs.open(filename, 'wb', 'UTF-8') as f:
		f.write(template)
		f.flush()

	print('[created] ' + filename)

def createLayout(config):
	layoutName = config['layout_name']
	layoutName = config['layout_dir'] + layoutName + '.xml'

	layout = ''
	with codecs.open('layout.xml', 'rb', 'UTF-8') as f:
		layout = f.read()
	if not layout:
		return
	with codecs.open(layoutName, 'wb', 'UTF-8') as f:
		f.write(layout)
		f.flush()

	print('[created] ' + layoutName)

def main():
	configs = {}
	with codecs.open('config.json', 'rb', 'UTF-8') as f:
		configs = json.loads(f.read())
	if not configs:
		return

	createTime = time.strftime('%Y-%m-%d',time.localtime(time.time()))

	for config in configs:
		config['create_time'] = createTime
		if config['is_need_activity']:
			create('Activity.java', 'Activity', config, '/activity/')
		createLayout(config)
		if config['is_list']:
			create('ListContract.java', 'Contract', config, '/contract/')
			create('ListFragment.java', 'Fragment', config, '/view/')
			create('ListPresenter.java', 'Presenter', config, '/presenter/')
			create('VH.java', 'VH', config, '/view/viewholder/')
			create('Bean.java', '', config, '/model/bean/')
		else:
			create('NormalContract.java', 'Contract', config, '/contract/')
			create('NormalFragment.java', 'Fragment', config, '/view/')
			create('NormalPresenter.java', 'Presenter', config, '/presenter/')

if __name__ == "__main__":
	main()
```