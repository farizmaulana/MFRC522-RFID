# MFRC522-RFID
import cv2
import datetime
import pysftp as sftp
import serial
import string
import time
import urllib
import urllib2

gantryID = 'ERPGANTRY001'
cardID = 'ERPITB0416000001'
obuID = '1ERP0001'
host = '188.166.247.181'
username = 'serverpilot'
password = 'electronicroadpricing'
port = 5010
local_dir ='/home/fariz/Learning/python/image/'
remote_dir = 'data/image'

def captureVehicle():
	camera = cv2.VideoCapture(1)
	retval, image = camera.read()
	path = local_dir + 'image.jpg'
	
	cv2.imwrite(path, image)
	
	return path;

def uploadImageToServer(host, username, port, file):
	try:
		server = sftp.Connection(host = host, username = username, password = password, port = port)
		with server.cd('remote_dir'):
			server.put()
		server.close()
	except Exception, e:
		print str(e)

def receiveDataFromOBU():
	ser = serial.Serial(port='/dev/ttyAMA0', baudrate=9600)
	while True:
		ser.read(Data)
		
def sendDataToServer(gantryID, cardID, obuID):
	url = 'http://serverphp.dilantai4pau.com/android_api/transaction.php?%s'
	params = { 	'gantry_serial':gantryID, 
				'card_id':cardID, 
				'obu_serial':obuID	}
	encoded_params = urllib.urlencode(params)
	request = urllib.urlopen(url % encoded_params)
	response = request.read()

receiveDataFromOBU()
sendDataToServer(gantryID, cardID, obuID)
uploadImageToServer(host, username, port, file)
