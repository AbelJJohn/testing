# Build, Run, Test Instructions

***Build:***
`docker build -t cs-abel-john-file-metadata:0.0.1 .`

***Run:***
`docker run -it -v $(pwd):/app cs-abel-john-file-metadata:0.0.1`

***Test:***
`docker run -it -v $(pwd):/app cs-abel-john-file-metadata:0.0.1 sh -c pytest`


# Python vs Pyspark Solutions

I approached and solved this assignment in two separate ways: Pure python and Pyspark. Each have their own strengths and weaknesses. 
I will address the reason I am submitting the pure pythonic solution in the following sections. 

I can provide the Pyspark solution (Dockerfile, python file, readme on how to build) on request.

## Goal of the assignment
Provide a Dockerfile and a python file that contains information about the sample files in a time/space efficient manner.

## Time and Space Complexity
The python solution is able to run the necessary analysis of the files, after the data has been processed, in O(N) time, where N is the 
size of the files in bytes. The data process in the above statement takes O(N^2) time. This complexity is the same in Pyspark but the process will
be parallelized among multiple cores.

The python space complexity is O(N) as each file is received as bytes from the python request for analysis. Pyspark uses Resilient Distributed Datasets 
(RDDs)as its primary data structure. As such, the space complexity would be O(N/k) where k is the number of machines/cores in the cluster.

## Docker Build Time
***Python:***
The build process for the Python docker container is simple and straightforward. I use a basic python3.6.5-alpine3.7 image as the base, which is
quite small and can spin up quite quickly. I would additionally need to install just two pip packages before the code can be run in the container.
The final docker image size is expected to be ~95MB.

***Pyspark:***
The build process for the Pyspark container, however, is much more convoluted. Pyspark is built on Spark's Java API and uses py4J to initialize a
JVM for SparkContext. Spark requires JDK-8 to be in the file path. In order to accomodate all of these requirements, the dockerfile for the Pyspark
solution is much more complicated. First, the base image is Ubuntu on which I install JDK-8 and Spark. Both of these installations are time intensive 
as JDK-8 is ~100MB and Spark with Hadoop is ~150MB. This can take quite a long time to download so the container build time becomes extremely
dependent on the network connection.  The final docker image is expected to be ~1.5GB which is FAR too large for a problem of this scale.

## What about scale?
Scale is the primary reason that I considered a Pyspark solution. The current requirements only have files that are <1MB each so the pure python
solution running at O(N) has no real issue with time. In fact, the Python solution is faster than the Pyspark solution at the current scale as Pyspark
has a loading time. As the scale of this problem increases to the gigabyte scale, however, the map-reduce power of Pyspark comes into play. Pyspark is
able to parallelize the tasks across the logical cores of the system. This means that getting the frequency of total and unique words in the files 
would be easily parallilazable even at a large scale, resulting in a faster solution.

## Final thoughts
Given the scale of the problem, the space/time complexitiy, and the size of the docker image, the python solution is more fitting for this assignment.

# Testing

Extra test sample files found at: https://github.com/AbelJJohn/testing/tree/main/sample_files

asdfkj_file_aaa.txt = Incorrectly renamed sample_file_0.txt

sample_file_1000.txt = 250x Original sample_file_0.txt. Size is 93MB.

sample_file_3.lst = Incorrectly renamed sample_file_18.txt

sample_file_5space.txt = 5 spaces

sample_file_ab2.txt = Renamed sample_file_0.txt

sample_file_one.txt = One long word

## Test Cases

1. Checking data transmission accuracy. Since I am using requests to get the data from github, prove the data is accurate.

2. Checking input file naming style. The requirement wants metadata only from `sample_file_\*.txt`. Show that this is resolved in the code.

3. Checking data availability. If requests fails to get a response due to network time out, either run the request again or log connection issue.

4. Checking data quality. If a file is empty, one large word, or corrupt, ensure that the program will work as intended.
