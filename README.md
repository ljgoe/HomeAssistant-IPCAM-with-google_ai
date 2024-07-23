<a name="readme-top"></a>
<!--

<!-- PROJECT LOGO -->
<br />
<div align="center">
  <a href="https://testit.vc/">
    <img src="Images/logo.png" alt="Logo" width="80" height="80">
  </a>
  <h3 align="center">Home Assistant with Google Generative AIl</h3>
</div>

When motion is detected on CCTV camera, Google Generative AI sends descitption of the snapshot to your phone
<p align="left">
    <img src="Images/Notification.jpg">
</p>
This is a quick guide your Home Assistant to take a snapshot when motion is detected on your IP Camera, have Google Generative AI Conversation integration to describe the snapshot, and send a notification with the snapshot and description.

# Requirements: 
1. Add your phone to HA by installing the app on your device and logging in.
2. Your cameras need to be added in HA, they will need to support motion detection and HA must have an entity to be able to see that motion alarm status.

# Getting Started
1. Install the integration from here: https://www.home-assistant.io/integrations/google_generative_ai_conversation/
2. Create a directory under your www folder called "snapshots"

<p align="left">
    <img src="Images/Snapshots.png" width="450" height="400">
</p>

3. Ensure your camera has motion detection turned on and that HA can see the alarm when motion is detected.

4. Create a AUTOMATION named: "Driveway - Snapshot on motion" using the following code: [ai_notifications_automation](https://github.com/ljgoe/HomeAssistant-IPCAM-with-google_ai/blob/main/ai_notifications_automation) Note that switching to visual editor will help you find your camera and entity id's. This automation will run the script (created in the next step) when HA receives the motion alarm from your camera. Choose Edit in Yaml in the top right corner
<p align="left">
    <img src="Images/Automation1.png">
      <img src="Images/Edit_in_Yaml.png">
</p>

5. Create a SCRIPT called: "Driveway - Snapshot, AI & Notification" with the following code: [https://github.com/ljgoe/HomeAssistant-google_ai/blob/main/ai_notification_script](https://github.com/ljgoe/HomeAssistant-IPCAM-with-google_ai/blob/main/ai_notification_script) This script takes the snapshot, gets Google AI to describe it and then send you the notification with what the AI sees and with snapshot image attached. You can run this manually to test. :)
<p align="left">
    <img src="Images/Scripts.png">
</p>

You should now receive a notification describing the image when your camera detects motion and takes a snapshot. This should give you an idea of how it works. You can edit the script’s prompt to get more accurate responses and duplicate these automations and scripts for as many cameras as you need.

This one takes 3 snapshots 0.5 seconds apart, sends those 3 images google to compare and if google notices no significant motion, the notification will not be delivered.

This reduces false notifications from moving objects like trees and helps the AI detect significant movements such as cars. Note that if you modify the Google prompt “If you see no obvious causes of motion, reply with ‘Camera has detected motion however no obvious motion observed comparing snapshots’,” you will also need to adjust the corresponding IF statement accordingly.

Note: This script sends 3 images to Google. If you encounter the “Error generating content: 400 Request payload size exceeds the limit” error, it means your camera snapshots are too large. To resolve this, manually run the script and adjust it to send only 2 images. You can do this by removing the last snapshot action, the preceding 0.5-second delay, and the reference to the 3rd image under image_filename in the Google AI service section.
