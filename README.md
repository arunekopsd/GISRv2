# GISR – Gamified Intervention for Stress Relief

A web app that tracks how a player's emotions change while they play a game. A facial emotion recognition model reads the player's expressions through the webcam before, during and after a play session, and the app reports how their mood shifted.

Built by Team 12 as a college project on gamified interventions for stress and depression.

## The idea

Traditional stress and depression support can feel clinical and is easy to drop. GISR explores whether a game can help instead, by reacting to how the player feels rather than playing the same way for everyone.

The long-term goal is **adaptive difficulty as positive reinforcement**: the game gets easier when the player looks sad, giving them small wins to lift their mood, and harder when they look happy, keeping them engaged.

This version builds the foundation for that: real-time emotion recognition, and a way to measure whether a play session actually changes the player's mood. The original proposal planned to read emotions from an EEG headset and wearables; this version uses the webcam instead, so it runs on any PC with a camera.

## How a session works

1. **Sign up or log in.** Accounts are handled by Firebase Authentication.
2. **Take the quiz.** 11 multiple-choice questions, each answer linked to happiness, sadness, fear or anger. This gives a starting picture of the player's mood.
3. **Pre-game (30 s).** Emotion recognition starts in a webcam window, recording a baseline before play. A live chart shows the emotion counts.
4. **Play (60 s).** The app launches the game, and emotions are tracked while the player plays.
5. **Post-game (30 s).** A final round of recording to see how the player feels after playing.
6. **Session report.** A donut chart of the quiz results, plus a bar chart for each phase, showing how the player's emotions changed over the session.

Every phase's results are saved per user in the Firebase Realtime Database.

## Emotion recognition

- **Face detection:** OpenCV's Haar cascade finds faces in each webcam frame.
- **Classification:** Each face is cropped, converted to 48×48 grayscale and passed to a convolutional neural network built with Keras. It predicts one of seven emotions: angry, disgust, fear, happy, neutral, sad or surprise.
- **Tracking:** Predictions are counted per emotion; disgust is detected but not counted. The browser polls the counts every few seconds to update the live chart and save them.

## Tech stack

- **Backend:** Python, Flask
- **ML / vision:** TensorFlow, Keras, OpenCV
- **Auth and database:** Firebase (via Pyrebase)
- **Frontend:** HTML, Tailwind CSS, Alpine.js, Chart.js

## Running it locally

The app opens the webcam and launches a game as a Windows executable, so it's meant to run on a Windows PC with a webcam.

**1. Install dependencies** (Python 3.10 recommended; the pinned TensorFlow and pandas versions need 3.8–3.10)

```bash
git clone https://github.com/arunekopsd/GISRv2.git
cd GISRv2
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

**2. Add the files that aren't in the repo**

The trained model and the game aren't included in this repository. Place them like this:

```
model/emotiondetector.json   model architecture
model/emotiondetector.h5     model weights
game/StackOBot.exe           packaged Unreal Engine game the app launches
```

**3. Set up Firebase**

Create a Firebase project with Email/Password authentication and a Realtime Database. Copy `.env.example` to `.env` and fill in your project's values.

**4. Run**

```bash
python app.py
```

Open http://localhost:5000. Press **Esc** in the webcam window to stop emotion recognition.

## Project structure

```
app.py              Flask routes, quiz scoring, emotion recognition
templates/          pages: quiz, game phases, report, login/signup
static/             background image
requirements.txt    Python dependencies
.env.example        Firebase and Flask settings template
```

## Team

- Farhan Naeem
- [Ashwin Sivasankaran](https://github.com/ashwinsk24)
- Catherine Jose
- Bimal Dev S

Forked from [ashwinsk24/GISRv2](https://github.com/ashwinsk24/GISRv2).
