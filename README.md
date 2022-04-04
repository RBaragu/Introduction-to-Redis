# Introduction-to-Redis
#Installing redis library that lets you talk to a Redis server directly through Python calls
!pip install redis
Collecting redis
  Downloading redis-4.1.4-py3-none-any.whl (175 kB)
     |████████████████████████████████| 175 kB 5.2 MB/s 
Requirement already satisfied: importlib-metadata>=1.0 in /usr/local/lib/python3.7/dist-packages (from redis) (4.11.0)
Requirement already satisfied: packaging>=20.4 in /usr/local/lib/python3.7/dist-packages (from redis) (21.3)
Collecting deprecated>=1.2.3
  Downloading Deprecated-1.2.13-py2.py3-none-any.whl (9.6 kB)
Requirement already satisfied: wrapt<2,>=1.10 in /usr/local/lib/python3.7/dist-packages (from deprecated>=1.2.3->redis) (1.13.3)
Requirement already satisfied: typing-extensions>=3.6.4 in /usr/local/lib/python3.7/dist-packages (from importlib-metadata>=1.0->redis) (3.10.0.2)
Requirement already satisfied: zipp>=0.5 in /usr/local/lib/python3.7/dist-packages (from importlib-metadata>=1.0->redis) (3.7.0)
Requirement already satisfied: pyparsing!=3.0.5,>=2.0.2 in /usr/local/lib/python3.7/dist-packages (from packaging>=20.4->redis) (3.0.7)
Installing collected packages: deprecated, redis
Successfully installed deprecated-1.2.13 redis-4.1.4

#Importing redis
import redis
r = redis.StrictRedis(
    host='redis-11221.c1.asia-northeast1-1.gce.cloud.redislabs.com',
    port=11221,
    password='NcAnQzXMRLN9qbki93m9Co5IZgsKNGER',
    db=0,
    decode_responses=True
)

#Upload csv file
import csv
file = open("/content/dummy.csv",encoding='ISO 8859-1')

header= next(file)

#reading csv file
csvreader = csv.reader(file)

#Create an empty list called rows and iterate through the csvreader object and 
#append each row to the rows list.

rows = []
for row in csvreader:
        rows.append(row)
rows

# We will use json for the bulk upload since the `set` redis command 
#overwrites data inserted individually
import json

# Define a variable for the redis documents key
key = "dummy"
#Inserting our details to the database
r.set(key, json.dumps(rows))
True

import pandas as pd
#defining pandas data frame
df =  pd.DataFrame()

#downloading db and storing it in a variable db
db = json.loads(r.get(key))
#insert dataframe
for item in db:
  df = df.append(item, ignore_index=True)
  
  Updating Database
  # Read in the data from the telephone csv file
file = csv.reader(open("/content/telephone update.csv",encoding='ISO 8859-1'))
header = next(file)

# Save the data in a new list
rows_update= []

for row in file:
  f = dict(zip(header, row))
  rows_update.append(f)


# Combine the previous list with the new
updated = rows_update + rows

# Upload the new list to the database
r.set(key, json.dumps(updated))

Delete the same data from the Redis database
# Delete the data
r.flushdb()

# Confirm deletion
print(r.keys())
