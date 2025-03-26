

Selamat siang Mba.... Saya ingin konfirmasi untuk popup otp, wording "Silakan menunggu selama 2 menit." tidak ada di respons apakah harus di hardcode dari FE? 

berikut responsnya, 
```json
{
  "errorCode": "0044",
  "sourceSystem": "Digital Banking",
  "message": "maximum resend",
  "transactionId": null,
  "engMessage": " You reached maximum resend OTP attempts",
  "idnMessage": "Anda mencapai batas maksimum kirim ulang kode OTP",
  "activityRefCode": null,
  "refNumber": null,
  "toAccountNumber": null,
  "failedPinAttempts": 0,
  "settingFailedPinAttempts": 0,
  "failedPasswordAttempts": 0,
  "failedSofTokenAttempts": 0
}```


```json
{
  "errorCode": "0078",
  "sourceSystem": "Digital Banking",
  "message": "3",
  "transactionId": null,
  "engMessage": "You have reached the maximum number of generate for OTP Please wait for 3 minutes.",
  "idnMessage": "Anda telah mencapai batas maksimum generate kode OTP. Silakan menunggu selama 3 menit.",
  "activityRefCode": null,
  "refNumber": null,
  "toAccountNumber": null,
  "failedPinAttempts": 0,
  "settingFailedPinAttempts": 0,
  "failedPasswordAttempts": 0,
  "failedSofTokenAttempts": 0
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ4ODA4NzE0NSw4MzczNzMxMzBdfQ==
-->