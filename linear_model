##from csv import DictReader, writer
import csv
import time, datetime, math
from sklearn import linear_model
import numpy as np
Start = datetime.datetime.now()

train = "train.csv" #Path to training data
test = "test.csv" # Path to test dataset

Id, Y_tst = [], []

# Function to extract time related variables
def dataprep(row):
## Start data modification here  
    # Extract date related variables from datetime # 2011-01-02 17:00:00
    # time.struct_time(tm_year=2011, tm_mon=1, tm_mday=1, tm_hour=0,
    # tm_min=0, tm_sec=0, tm_wday=5, tm_yday=1, tm_isdst=-1)
    row['year'] = time.strptime(row['datetime'], "%Y-%m-%d %H:%M:%S")[0]
    row['hour'] = time.strptime(row['datetime'], "%Y-%m-%d %H:%M:%S")[3]
    row['month'] = time.strptime(row['datetime'], "%Y-%m-%d %H:%M:%S")[1]
    row['day'] = time.strptime(row['datetime'], "%Y-%m-%d %H:%M:%S")[2]
    row['weekday'] = time.strptime(row['datetime'], "%Y-%m-%d %H:%M:%S")[6]
## End of modifications
    return row

def build_model(X_trn, Y_trn):
    # Model building part
    model = linear_model.LinearRegression()
    model.fit(X_trn, Y_trn)
    print model.coef_
    MSE = np.mean((model.predict(X_trn) - Y_trn) ** 2)
    print MSE
    # explained variance(R^2)
    print model.score(X_trn, Y_trn)
    return(model)

# Prepare training data
for yr in [2011, 2012]:
    for mnth in xrange(1,13):
        # Start here
        X_trn, X_tst = [], [] # array of features
        Y_trn = [] # target variable, output_index

        for t, row in enumerate(csv.DictReader(open(train))):
            row = dataprep(row)
            del row['datetime'] # we don't need datetime string
            del row['registered'] # info not present in test
            del row['casual'] # info not present in test data
            if row['month'] <= mnth and row['year'] <= yr:
                Y_trn.append(math.log(int(row['count']))) # collect target variable
                del row['count']
                del row['year']
                del row['month']
                # collect data lables
                labels_trn = row.keys()
                val = row.values()
                val = [float(x) for x in val]
                X_trn.append(val)

        model = build_model(X_trn, Y_trn)

        # Prepare test data
        for t, row in enumerate(csv.DictReader(open(test))):
            row = dataprep(row)
            if row['month'] == mnth and row['year'] == yr:
                Id.append(row['datetime'])
                del row['datetime']
                del row['year']
                del row['month']
                # collect data lables
                labels_tst = row.keys()
                # Change variables to integers and append
                val = row.values()
                val = [float(x) for x in val]
                X_tst.append(val)

        if labels_trn == labels_tst:
            print "Same variables used"
            y = model.predict(X_tst)
            y = [int(math.exp(x)) for x in y]
            Y_tst.extend(y)
        else: raise NameError("Some variables aren't matched")
            
        
print len(Id), len(Y_tst)
#Write results to a csv file
with open('predict.csv','w') as csvfile:
        writer = csv.writer(csvfile, quoting=csv.QUOTE_MINIMAL)
##        writer.writerow(['datetime', 'count']
        writer.writerows(zip(Id, Y_tst))
End = datetime.datetime.now()
print End - Start




























