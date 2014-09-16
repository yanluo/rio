#Kismet log parser

try:
	import xml.etree.cElementTree as ET
except ImportError:
	import xml.etree.ElementTree as ET
import time
import os

#Define the lists
name = [] 	
addr = []
dbm = []
ft = []
ft_c = []
lt = []
lt_c = []
In = [0]*100
Out = [0]*100
counter = []
index = -1
index_j = 0
index_k = 0

#find '.netxml' files in the directroy
file_name = ''
filelist = os.listdir('/home/roy/Roy/parser/2')    #according to the real directory
filelist.reverse()
for f in filelist:
	if f[-7:] == '.netxml':
		file_name = f

		#Open log file
		tree = ET.ElementTree(file=file_name)

		#Get the root node
		root = tree.getroot()
	
		for elem in tree.iter(tag = 'client-manuf'):
			name.append(elem.text)
	                index += 1
		counter.append(index)
		for elem in tree.iter(tag = 'client-mac'):
			addr.append(elem.text)
		for elem in tree.iterfind('wireless-network/wireless-client/snr-info/last_signal_dbm'):
			dbm.append(elem.text)
		for elem in root.findall('.//wireless-client'):
			ft.append(elem.get('first-time'))
			lt.append(elem.get('last-time'))
#Check the distance
#for i in range(len(dbm)):
#	if(dbm[i] <= ''):
#		name[i] = ''
#		addr[i] = ''
#		ft[i] = ''
#		l[i]t = ''
#		dbm[i] = ''
	

# Find the first-time and last-time for the same device
for j in range(len(addr)):
	for k in range(j+1,len(addr)):	
		if(addr[j] == addr[k] and addr[j] != ''):
			for l in range(len(counter)):
				if (counter[l] < j <= counter[l+1]):	
					index_j = l 
				if (counter[l] < k <= counter[l+1]):	
					index_k = l	
			if(index_j <= index_k +4 and index_j != index_k):
				ft[k] = ''
				lt[j] = ''
				dbm[k] = ''	
			elif(index_j == index_k):	
				ft[k] = ''
				lt[k] = ''
				dbm[k] = ''	
						
#Change date format so that it can be compared
for i in range(len(ft)):
	if(ft[i] != ''):
		ft_c.append(time.mktime(time.strptime(ft[i],'%a %b %d %H:%M:%S %Y')))
	if(lt[i] != ''):
       		lt_c.append(time.mktime(time.strptime(lt[i],'%a %b %d %H:%M:%S %Y')))

#Find the client with the same mac address      
for j in range(len(addr)):
	for k in range(j+1,len(addr)):	
		if(addr[j] == addr[k] and addr[j] != ''):
			addr[k]=''               #if the mac address is repeated, set it with ''
			name[k]=''
			if(ft[k] == ''):
				ft[j] += ft[k]
			else:		
	                	ft[j] += "\t"+ft[k]
       	                	ft[k] = ''
			if(lt[j] == ''):
				lt[j] += lt[k]
				lt[k] = ''
       	                else:
				lt[j] += "\t"+lt[k]
       	                	lt[k] = ''
			if(dbm[k] == ''):
       	                	dbm[j] += dbm[k]
			else:
				dbm[j] += "\t\t\t\t"+dbm[k]
       	                	dbm[k] = ''

#Remove '' in the lists
name = filter(lambda x:x !='',name)
addr = filter(lambda x:x !='',addr)
ft = filter(lambda x:x !='',ft)
lt = filter(lambda x:x !='',lt)
dbm = filter(lambda x:x !='',dbm)
	
#Print the results
print '='*128
for i in range(len(name)):
	print 'Number: ',i+1	
	print 'Name: ',name[i]
	print 'Mac: ',addr[i]
	print 'First-time: ',ft[i]
       	print 'Last-time:  ',lt[i]
	print 'Last_signal_dbm: ', dbm[i]
	print '='*128

#Print the statistics
t1 = time.mktime(time.strptime("Thu Sep 4 13:03:30 2014",'%a %b %d %H:%M:%S %Y'))
t2 = time.mktime(time.strptime("Thu Sep 4 13:06:00 2014",'%a %b %d %H:%M:%S %Y'))
t3 = time.mktime(time.strptime('Thu sep 4 13:07:00 2014','%a %b %d %H:%M:%S %Y'))

for x in range(len(ft_c)):
	 if(t1 < ft_c[x] <= t2):
		In[0] += 1 
         elif(t2 < ft_c[x] <= t3):
                In[1] += 1
         else:
	 	In[2] += 1

for x in range(len(ft_c)):
	 if(t1 < lt_c[x] <= t2):
		Out[0] += 1 
         elif(t2 < lt_c[x] <= t3):
                Out[1] += 1
         else:
	 	Out[2] += 1
print '\t\t\t\t\t\t\tIN\tOUT'
print 'Thu Sep 4 13:03:30 2014--Thu Sep 4 13:06:00 2014\t',In[0],'\t',Out[0]
print 'Thu Sep 4 13:06:00 2014--Thu Sep 4 13:07:00 2014\t',In[1],'\t',Out[1]
print 'Thu Sep 4 13:07:00 2014--Thu Sep 4 13:09:00 2014\t',In[2],'\t',Out[2]

                

