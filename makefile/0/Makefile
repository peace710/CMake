file0.txt:
	echo "foo" > file0.txt

file1.txt:
	@cat file0.txt > file1.txt

file2.txt file3.txt: file0.txt file1.txt
	touch file2.txt
	touch file3.txt
