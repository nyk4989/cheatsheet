1. **Goのインストール**
	katanaはGoで書かれているのでGoのインスールは必須
	```sh
	sudo apt update
	sudo apt install golang-go

	# Goが入っているかの確認
	go version
	# これ必要かも？
	echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.zshrc # ←zshのShellを使用している場合は.zshrcにする必要がある。
	source /home/<ユーザ名>/.zshrc
	```
2. **katanaのインストール** 
	```sh
	# インストール
	go install github.com/projectdiscovery/katana/cmd/katana@latest
	# PATHjを通す
	echo 'export PATH=$PATH:$HOME/go/bin' >> ~/.zshrc
	source /home/<ユーザ名>/.zshrc
	```
3. 動作確認
	```sh
	katana -h
	```