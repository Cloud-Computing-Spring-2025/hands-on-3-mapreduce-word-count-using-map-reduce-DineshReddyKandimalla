Project Overview:
This project implements a Word Count program using Hadoop MapReduce. It processes a given text file, counts the occurrences of each word, and outputs the results in descending order of frequency.

Approach and Implementation

Mapper Logic: 
 Reads each line from the input file.
 Splits the line into words using a StringTokenizer.
 Emits (word, 1) pairs for each word.

Example Output from Mapper:

hello 1 
world 1 

Reducer Logic:
 Receives (word, [1, 1, 1, ...]) as input from the mapper.
 Sums up the values to get the total count for each word.
 Emits (word, count) as output.

Example Output from Reducer:
hello 2

Execution Steps:

Start Hadoop Cluster: docker compose up -d

Build the Project Using Maven: mvn install

Move JAR File to Shared Folder: mv target/*.jar shared-folder/input/data/

Copy JAR to Docker Container: docker cp shared-folder/input/data/WordCountUsingHadoop-0.0.1-SNAPSHOT.jar resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/

Copy Dataset to Container: docker cp shared-folder/input/data/input.txt resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/

Access ResourceManager Container: docker exec -it resourcemanager /bin/bash cd /opt/hadoop-3.2.1/share/hadoop/mapreduce/

Setup HDFS Directories:

hadoop fs -mkdir -p /input/dataset hadoop fs -put ./input.txt /input/dataset

Run the MapReduce Job:

hadoop jar WordCountUsingHadoop-0.0.1-SNAPSHOT..jar com.example.controller.Controller /input/dataset/input.txt /output

View Output:

hadoop fs -cat /output/*

Copy Output to Local Machine:

hdfs dfs -get /output /opt/hadoop-3.2.1/share/hadoop/mapreduce/ docker cp resourcemanager:/opt/hadoop-3.2.1/share/hadoop/mapreduce/output/ shared-folder/output/

Challenges Faced & Solutions

Hadoop Configuration Issues:

Challenge: Errors due to incorrect file paths or environment settings.

Solution: Verified Hadoop configurations and paths before running commands.

Dataset Copy Errors:

Challenge: Difficulty in transferring files between local machine and container.

Solution: Used Docker cp command carefully to manage file transfers.

Case Sensitivity and Special Characters:

Challenge: There was build failure issue and also the directory in commands was not correct leading to issues.

Solution: Corrected the errors in code and re ran mvn install command, and changed the commands with correct directory.
Sample Input and Output

Input File (input.txt)

Hadoop is a framework that allows for the distributed processing of large data sets across clusters of computers. 
Hadoop is designed to scale up from single servers to thousands of machines, each offering local computation and storage. 
Rather than rely on hardware to deliver high-availability, the library itself is designed to detect and handle failures at the application layer. 

Expected Output:

a 1 across 1 allows 1 and 2 application 1 at 1 clusters 1 computation 1 computers 1 data 1 deliver 1 designed 2 detect 1 distributed 1 each 1 failures 1 for 1 framework 1 from 1 hadoop 2 handle 1 hardware 1 highavailability 1 is 3 itself 1 large 1 layer 1 library 1 local 1 machines 1 of 3 offering 1 on 1 processing 1 rather 1 rely 1 scale 1 servers 1 sets 1 single 1 storage 1 than 1 that 1 the 3 thousands 1 to 4 up 1

