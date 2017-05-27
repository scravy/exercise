# Simplaex Producer/Consumer Task

At simplaex we are processing huge amounts of data in realtime.
Your task is to write a small service that receives and analyzes
CSV data in real time.

You are free to do this exercise in any programming language /
on any platform you see fit (Java, Scala, Python, Node, Haskell, go crazy).

The service is supposed to listen on port 9000, you are free to
choose either TCP or UDP (for simplicity). Data comes in as
Comma Seperated Values, one row per line:

    0977dca4-9906-3171-bcec-87ec0df9d745,kFFzW4O8gXURgP8ShsZ0gcnNT5E=,0.18715484122922377,982761284,8442009284719321817

1. A UUID which identifies a user account
2. Some Base 64 encoded data
3. A floating point value
4. An integer
5. Another integer

For every thousand messages the service receives it should create a file with
the analysis of those 1000 messages. We are interested in:

1. the sum of data point 5 overall
2. the number of unique users
3. the average value of data point 3 (the floating point value) per user
4. the most recent value of data point 4 per user

The results should be written in CSV format, i.e.

    982374938509546879632492840347
    687
    0977dca4-9906-3171-bcec-87ec0df9d745,4.92308045,982393
    
that is the first line should contain the sum of datapoint 5,
the second line should contain the number of unique users,
and the remaining lines should each contain the unique user id,
the average value and the most recent value as defined above.

It is fine for the files produced to be named just with the
current timestamp in milliseconds or a running number (i.e.
1.txt, 2.txt, 3.txt, and so on).

Checklist:

1. Read messages on port 9000, one per line
2. For each thousand messages: Aggregate the above mentioned metrics
   in a CSV file

Hand in the results with a README instructing how to compile
and run your code as a ZIP or TAR.GZ file, or send the link
to a git repository.


## Running the producer

The producer needs Oracle Java 8.

    java -jar producer.jar --tcp
    
Runs the producer, sending data to port 9000 via TCP.

    java -jar producer.jar --udp
    
Runs the producer, sending data to port 9000 via UDP.

Play with it:

1. In a terminal run `nc -ul 9000` (this starts a process which listens
on UDP port 9000 and dumps everything it receives to stdout) or
`nc -l 9000` (same, but TCP).
2. Start the producer

You will see messages coming in which look like that:

```
0977dca4-9906-3171-bcec-87ec0df9d745,kFFzW4O8gXURgP8ShsZ0gcnNT5E=,0.18715484122922377,982761284,8442009284719321817
5fac6dc8-ea26-3762-8575-f279fe5e5f51,cBKFTwsXHjwypiPkaq3xTr8UoRE=,0.7626710614484215,1005421520,6642446482729493998
0e0094f2-04df-37d7-abba-1dc398403709,9ZWcYIblJ7ebN5gATdzzi4e8K7Q=,0.9655429720343038,237475359,3923415930816731861
4d968baa-fe56-3ba0-b142-be9f457c9ff4,RnJNTKLYpcUqhjOey+wEIGHC7aw=,0.6532229483547558,1403876285,4756900066502959030
99ccd24b-013a-3170-b70e-8d90a85d9775,N0fiZEPBjr3bEHn+AHnpy7I1RWo=,0.8857966322563835,1851028776,6448117095479201352
f76b9ef2-0054-3e6a-8e9c-ace15bcde68b,P/wNtfFfa8jIn0OyeiS1tFvpORc=,0.8851653165728414,1163597258,8294506528003481004
26c0dd58-b646-3c02-95c5-ebafee5e0f10,Aizem/PgVMKsulLGquCAsLj674U=,0.5869654624020274,1012454779,2450005343631151248
```

that is, CSV data, one row per line. The columns are
