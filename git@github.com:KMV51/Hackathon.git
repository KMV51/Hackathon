import smtplib
import cv2
import time

PatientID = 'BabyA_1234demo'

cam = cv2.VideoCapture(0)

while True:
	ret, image = cam.read()
	cv2.imshow('CameraView',image)
	k = cv2.waitKey(1)
	if k != -1:
		break
cv2.imwrite('/media/pi/92B05BEEB05BD6F7/' + PatientID + '.jpg', image)
cam.release()
cv2.destroyAllWindows()

from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from email.mime.image import MIMEImage

SMTP_SERVER = 'smtp.gmail.com'
SMTP_PORT = 587
GMAIL_USERNAME = 'testemailforhackathon@gmail.com'
GMAIL_PASSWORD = 'Testing123!'

class Emailer:
    def sendmail(self, recipient, subject, content, image):

        headers = ["From: " + GMAIL_USERNAME, "Subject: " + subject, "To: " + recipient,
                   "MIME-Version: 1.0", "Content-Type: text/html"]
        headers = "\r\n".join(headers)
        
        emailData = MIMEMultipart()
        emailData['Subject'] = subject
        emailData['To'] = recipient
        emailData['From'] = GMAIL_USERNAME

        emailData.attach(MIMEText(content))

        imageData = MIMEImage(open(image, 'rb').read(), 'jpg') 
        imageData.add_header('Content-Disposition', 'attachment; filename=' + PatientID + '.jpg')
        emailData.attach(imageData)

        session = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        session.ehlo()
        session.starttls()
        session.ehlo()
  
        session.login(GMAIL_USERNAME, GMAIL_PASSWORD)

        session.sendmail(GMAIL_USERNAME, recipient, emailData.as_string())
        session.quit
  
  
sender = Emailer()
image = '/media/pi/92B05BEEB05BD6F7/' + PatientID + '.jpg'
sendTo = 'sunnyvmakwana@gmail.com'
emailSubject = "Baby-A has taken a picture"
emailContent = "Baby-A (Patient_ID or other identifier in doctor's system) has taken a picture. The picture was taken at: " + time.ctime()
sender.sendmail(sendTo, emailSubject, emailContent, image)
print("Email Sent")
