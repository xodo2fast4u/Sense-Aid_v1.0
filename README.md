## Sense-Aid v1.0

Sense-Aid is a wearable head-mounted device that gives blind people an artificial eye. It uses specialized AI-equipped cameras running real-time object detection to describe their surroundings, all without requiring them to pull out a phone or ask for help.

### The Problem

Blind and visually impaired people navigate the world with remarkable determination. But there's a gap. Audio descriptions help, but they're incomplete and often come from other people. They need something reliable, immediate, and independent. They need to know what's in front of them right now.

Sense-Aid closes that gap.

### How It Works

The system runs on an ESP32 microcontroller paired with AI vision processing. Here's what happens when someone wears it:

1. The camera captures what's ahead
2. YOLOv3 object detection identifies objects in real time
3. The system converts detections to audio feedback through speakers
4. Ultrasonic sensors detect obstacles and trigger haptic feedback (vibration)

The result: immediate, hands-free awareness of the environment.

### Project Structure

ESP32 Code contains four main modules:

**yolov3** - The intelligence layer. This runs YOLOv3 object detection on camera input and identifies what's in the scene. It processes images and outputs class labels and confidence scores.

**yolov3_receive** - Receives the detection data from the YOLOv3 processor and acts as the communication bridge between detection and feedback systems.

**ble_speakers** - Handles Bluetooth Low Energy communication to send audio to connected speakers or headphones. Your detected objects get announced through audio feedback.

**ultrasonic_and_vibMotor** - Runs the safety layer. Ultrasonic sensors constantly scan for obstacles. When something gets too close, the vibration motor activates to warn the wearer without blocking audio information.

### Hardware Requirements

Before you run this, you'll need:

1. ESP32 development board
2. Camera module compatible with your ESP32 (check your specific board docs)
3. Ultrasonic sensor (HC-SR04 or equivalent)
4. Vibration motor
5. Bluetooth speaker or headphones
6. YOLOv3 model files (yolov3.cfg and proper weights if not included)
7. COCO dataset names file (coco.names for object labels)

### Getting Started

First, set up your Arduino IDE:

1. Install the Arduino IDE if you haven't already
2. Add ESP32 board support through the board manager
3. Install necessary libraries for your specific components (BLE, ultrasonic, etc.)

### Running the Code

Start with the individual modules. Each ino file is self contained for testing:

**Test Ultrasonic and Vibration First** - Flash ultrasonic_and_vibMotor.ino to verify your sensors and motor work correctly before adding complexity.

**Test Bluetooth Next** - Flash ble_speakers.ino and pair your device to a speaker. Verify connectivity works.

**Run YOLOv3 Detection** - Flash yolov3.ino. This handles the computer vision piece. It will output detected objects. Make sure your camera is properly connected.

**Wire Everything Together** - Once all pieces work independently, integrate them. The final system coordinates all modules: sensing obstacles, detecting objects, and delivering feedback simultaneously.

### Audio Assets

The wav and mp3 audio folder contains sound files for audio feedback. These get played back when objects are detected or obstacles approach. You can customize these for different object types or warning levels.

### Configuration Notes

Before flashing, check each ino file for:

Pin assignments: Make sure GPIO pins match your actual wiring
Baud rate: Serial communication speed (usually 115200)
Sensor thresholds: Distance triggers for vibration, confidence thresholds for object detection
Audio volume: Adjust for safety and clarity in the field

### What You Should Know

This is v1.0, which means it works but it's not perfect. YOLOv3 detection might call something a cat when it's a dog. Obstacle detection has a range limit. Bluetooth connectivity depends on your environment. That's not failure, that's reality. It's still several orders of magnitude better than nothing.

The value is in the attempt. Blind people using this get information they didn't have before. They get independence they didn't have before. That's what matters.

### Troubleshooting

"Nothing's working" - Start smaller. Test each component individually first. Ultrasonic sensor alone, then camera alone, then integration. Isolation helps.

"Detection is too slow" - YOLOv3 is heavy. If your ESP32 can't keep up, look at frame rate settings. Maybe process every other frame instead of every frame.

"Bluetooth won't connect" - Check that ble_speakers.ino is running and advertises the correct service UUID. Verify your speaker is in pairing mode.

"Vibration motor doesn't trigger" - Verify ultrasonic sensor readings first. Check the distance threshold. Test the motor pin directly before assuming software failure.

### Next Steps

This project can go deeper. Consider adding:

Multiple camera angles for wider awareness
Machine learning optimization to run faster
Custom training on common indoor/outdoor obstacles
Battery optimization for all day wear
Mesh networking between multiple devices

### Contributing

If you improve something, fix something, or make something work better, share it. Good code lives in community. Document what you changed and why. Show your work.

### License

See the LICENSE file for terms.

Got questions? Build. Test. Learn.
