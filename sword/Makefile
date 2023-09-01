CLI=adm
RESOURCE_PATH=./resource
ASSETS_PATH=./resource/assets
SEPARATION_PATH=./separation
COMMON_PATH=./../common

all: build

# build a theme
# 构建主题
build: rebuild-dist combine build-separation-zip build-assets build-tmpl clean fmt

# clean old dist folder, create new dist folder.
# 删除旧的dist文件夹，新建新的dist文件夹
rebuild-dist:
	rm -rf $(ASSETS_PATH)/dist
	rm -rf $(ASSETS_PATH)/src
	mkdir $(ASSETS_PATH)/dist
	mkdir $(ASSETS_PATH)/dist/js
	mkdir $(ASSETS_PATH)/dist/css
	mkdir $(ASSETS_PATH)/src

	cp -Rf $(COMMON_PATH)/assets/* $(ASSETS_PATH)/src/
	cp -Rf $(RESOURCE_PATH)/sword/sword.css $(ASSETS_PATH)/src/css/combine/i_sword.css
	cp -Rf $(RESOURCE_PATH)/sword/blue@2x.png $(ASSETS_PATH)/src/css/
	cp -Rf $(RESOURCE_PATH)/sword/blue.png $(ASSETS_PATH)/src/css/

	cp -Rf $(ASSETS_PATH)/src/js/*.js $(ASSETS_PATH)/dist/js/
	cp -Rf $(ASSETS_PATH)/src/css/*.png $(ASSETS_PATH)/dist/css/

	cp -Rf $(COMMON_PATH)/pages/* $(RESOURCE_PATH)/sword/pages/
	cp -Rf $(COMMON_PATH)/pages/*.tmpl $(RESOURCE_PATH)/sword/pages/
	cp -Rf $(COMMON_PATH)/pages/components/*.tmpl $(RESOURCE_PATH)/sword/pages/components/
	cp -Rf $(COMMON_PATH)/pages/components/table/*.tmpl $(RESOURCE_PATH)/sword/pages/components/table/

	cp -Rf $(ASSETS_PATH)/src/css/fonts $(ASSETS_PATH)/dist/css/
	cp -Rf $(ASSETS_PATH)/src/img $(ASSETS_PATH)/dist/
	cp -r $(ASSETS_PATH)/src/fonts $(ASSETS_PATH)/dist/

	rm -rf $(SEPARATION_PATH)/public
	mkdir $(SEPARATION_PATH)/public

	cp -Rf $(RESOURCE_PATH)/assets $(SEPARATION_PATH)/public/assets
	cp -Rf $(RESOURCE_PATH)/sword/pages $(SEPARATION_PATH)/public/pages
	rm -rf $(SEPARATION_PATH)/public/assets/vendor

# combine frontend assets(js/css)
# 合并并压缩前端js/css
combine: remove combine-js combine-css

combine-js:
	$(CLI) merge js --hash=true --src=$(ASSETS_PATH)/src/js/all/ --dist=$(ASSETS_PATH)/dist/js/all.min.js
	$(CLI) merge js --hash=true --src=$(ASSETS_PATH)/src/js/all_2/ --dist=$(ASSETS_PATH)/dist/js/all_2.min.js
	$(CLI) merge js --hash=true --src=$(ASSETS_PATH)/src/js/components/form/ --dist=$(ASSETS_PATH)/dist/js/form.min.js
	$(CLI) merge js --hash=true --src=$(ASSETS_PATH)/src/js/components/tree/ --dist=$(ASSETS_PATH)/dist/js/tree.min.js
	$(CLI) merge js --hash=true --src=$(ASSETS_PATH)/src/js/components/treeview/ --dist=$(ASSETS_PATH)/dist/js/treeview.min.js
	$(CLI) merge js --hash=true --src=$(ASSETS_PATH)/src/js/components/datatable/ --dist=$(ASSETS_PATH)/dist/js/datatable.min.js
	cp -Rf $(ASSETS_PATH)/dist/js/* $(SEPARATION_PATH)/public/assets/dist/js/

combine-css:
	$(CLI) merge css --hash=true
	cp -Rf $(ASSETS_PATH)/dist/css/*.css $(SEPARATION_PATH)/public/assets/dist/css/

# use packr2 turn assets into a go file.
# 使用packr2工具将css/js等前端资源合并为一个go文件
# more: https://github.com/gobuffalo/packr
build-assets:
	$(CLI) bundle asset
	packr2 clean
# turn golang template files into a go file.
# 将所有golang模板文件(.tmpl)合并为一个golang文件
build-tmpl:
	$(CLI) bundle tpl -p=sword

# clean
# 整理生成后的文件夹
clean:
	rm -rf $(ASSETS_PATH)/src/*
	rm -rf $(RESOURCE_PATH)/sword/pages/*

# go fmt
# 对生成的go文件进行格式化
fmt:
	GO111MODULE=off go fmt ./...

# remove useless system files.
# 删除系统产生的无用文件，防止被一并合并到golang文件中
remove:
	find ./ -name ".DS_Store" -depth -exec rm {} \;

# build assets zip of separation theme.
# 构建分离主题资源文件压缩包
build-separation-zip:
	rm $(SEPARATION_PATH)/public.zip
	cd $(SEPARATION_PATH)/public && zip -r -q ./../public.zip . -x "*.DS_Store" -x "__MACOSX"

.PHONY: all build rebuild-dist combine combine-js combine-css build-assets build-tmpl clean fmt remove