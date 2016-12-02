deploy:
	git checkout dev
	jekyll build
	git add -A
	git commit -m "update source"
	cp -r _site/ /tmp/
	git checkout master
	rm -r ./*
	cp -r /tmp/_site/* ./
	git add -A
	git commit -m "deploy blog"
	git push origin master
	git checkout dev
	echo "deploy succeed"
	git push origin dev
echo "push dev"