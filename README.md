#How to run 

1) Before running, put your untar big-corpus



#Implementation of Neighborhood Score: MapReduce

 In KNeighborScores.java, according to input value of REPETITIONS, run runApplication() $(REPETITIONS) times, this method will act as driver and run the following steps:

##Step 1: Run first round MapReduce:

Start job1:

In LetterCountMapper.java, fetch the data from HDFS, for each line, map to <key, value>, key is letters, value is frequency of key letter in this line;

In LetterCountReducer.java, reduce the <key,value>, for each key letter, calculate the total frequency.

##Step 2: Deal with output of first round reduce output:

Read the output of 1st round MapReduce output from HDFS, then calculate the total number of letters by sum the second column;

Read the 1st round output again, for each line, get the letter score by calculating letterfrequency / totalfrequency;

Set the letter scores into configuration.

##Step 3: Run second round MapReduce:

Use new configuration which contains letter scores and K to start a new Job job2

Start job2:

In KNeighborCalculateMapper, fetch books from HDFS, for words in each line, get the neighbors of each word, then according to letter scores, calculate the neighbor scores. So after mapper, we have <key, value>, at which key is word, value is the neighbor score of these key word.

In KNeighborCalculateReducer, for each key, calculate the average of each key.

##Step 4: Finish program
