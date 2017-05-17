KDD’99 cup dataset is extensively used for the evaluation of anomaly detection methods. The dataset is built based on the data captured in DARPA’98 IDS evaluation program [4], prepared by Stolfo el al. The data contains connection records of tcpdump data, with each connection record of 100 bytes containing 41 features. Also, each connection is labelled as either normal or an attack. The attacks fall into one of four categories listed below.
a.	Denial of Service (DoS): An attacker makes a memory or computer resource too busy, so that it will be busy to handle a new legitimate request or denies access to a legitimate user. Eg: 
b.	User to Root Attack (U2R): This is a type of attack where an attacker gains access to a user account on a system whose password is usually obtained by dictionary attack/password sniffing etc which will in turn help him gain root access to the system. 
c.	Remote to Local Attack (R2L): User who does not have access to a system attempts to gain local access of a user of that machine.
d.	Probing Attack: Attempts to gather information about a network of computers and try to breach its security controls. Eg: Port Scanning.
Pre-processing Techniques 
Each connection record contains the following features.
The fields 0 to 2 has duration, protocol_type, service. The fields 3 to 40 contain the other features of a connection like, number of failed logins, number of files accessed, number of out bounds. The 41st field contains the type of connection i.e. normal/ attack.
The pre-processing technique that we have used for this data is that we have removed all the fields of a record which are non-numeric and considered only the numeric fields for the analysis.
Approach 1:
We have used logistic regression model. We have trained the model using training data. Then we have used this model to predict the labels of test data. The results obtained are listed in table(a).
Approach 2:
For a more efficient model for classification we used feature selection. For this purpose, we have used the correlation between fields. There are many possible choices for feature selection in our model. We can select different combinations of correlated variables and just leave one of them. In our data, we have used the following combinations of correlated variables.
•	We know that dst_host_same_src_port_rate can be obtained from the other two fields src_bytes and srv_count where src_bytes are the number of bytes sent from source to destination and src_count is the number of connections made to the same service in the past two seconds as the current connection. So, we have dropped dst_host_same_src_port_rate and retained the other two.
•	Similarly, serror_rate and srv_error_rate is highly similar. So, we have dropped srv_error_rate. 
•	Using the same correlation principle we have dropped the following columns.
dst_host_same_src_port_rate, (column 35).
srv_serror_rate (column 25).
srv_rerror_rate (column 27).
dst_host_srv_serror_rate (column 38).
dst_host_srv_rerror_rate (column 40).
A new model is created after dropping the above columns. The accuracy is calculated on the test data and displayed on the table (a).
Approach 3:
To determine if a result is statistically significant Hypothesis testing can be used. To perform feature selection, we use an RDD of LabeledPoint. MLib internally calculates a contingency matrix and will perform a Chi Square test. Few features which don’t affect the accuracy are removed. A new model is created after dropping the columns. 
