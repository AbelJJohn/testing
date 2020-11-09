# Build and Run Instructions
1.`docker build -t cs-abel-john-file-metadata:0.0.1 .`
2.`docker run -it -v $(pwd):/app cs-abel-john-file-metadata:0.0.1`
3.`cat interview.csv`

# Python vs Pyspark Solutions

I approached and solved this assignment in two separate ways - each with their own strengths and weaknesses.

# Testing

Test files found at: https://github.com/AbelJJohn/testing/tree/main/sample_files

asdfkj_file_aaa.txt = Incorrectly renamed sample_file_0.txt
sample_file_1000.txt = 250x Original sample_file_0.txt
sample_file_3.lst = Incorrectly renamed sample_file_18.txt
sample_file_5space.txt = 5 spaces
sample_file_ab2.txt = Renamed sample_file_0.txt
sample_file_one.txt = One long word
