# Smart-QR-Based-Attendance-Management-System
# QR Code Generation (for each student)
# qr_generate.py
import qrcode

def generate_qr(student_id):
    img = qrcode.make(student_id)
    img.save(f"{student_id}_qr.png")
    print(f"QR Code generated for Student ID: {student_id}")

# Example usage
generate_qr("STUDENT123")
#QR Code Scanning and Attendance Marking
# qr_scan_attendance.py
import cv2
from pyzbar.pyzbar import decode
import csv
from datetime import datetime

def mark_attendance(student_id):
    with open('attendance.csv', 'a', newline='') as file:
        writer = csv.writer(file)
        now = datetime.now()
        writer.writerow([student_id, now.strftime('%Y-%m-%d'), now.strftime('%H:%M:%S')])
        print(f"Attendance marked for {student_id}")

def scan_qr():
    cap = cv2.VideoCapture(0)
    print("Scanning... Press 'q' to quit.")
    
    while True:
        success, frame = cap.read()
        for code in decode(frame):
            student_id = code.data.decode('utf-8')
            print(f"QR Code detected: {student_id}")
            mark_attendance(student_id)
            cap.release()
            cv2.destroyAllWindows()
            return
        cv2.imshow("QR Code Scanner", frame)
        if cv2.waitKey(1) & 0xFF == ord('q'):
            break
            
    cap.release()
    cv2.destroyAllWindows()
    # attendance.csv (Created automatically)
    student_id,date,time
STUDENT123,2025-05-15,09:05:30


# Run scanner
scan_qr()


