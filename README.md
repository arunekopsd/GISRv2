# GISR – Gamified Intervention for Stress Relief

A web app that studies how playing a game affects your emotions. While you play, your webcam feeds a facial emotion recognition model that tracks how you feel before, during and after the game. At the end you get a report comparing the three phases with a short emotion quiz you take at the start.

Built by Team 12 as a college project.

## How a session works

1. **Sign up or log in.** Accounts are handled by Firebase Authentication.
2. **Take the quiz.** 11 multiple-choice questions, each answer linked to happiness, sadness, fear or anger. The app shows your dominant emotion and saves the scores.
3. **Pre-game (30 s).** Emotion recognition starts in a webcam window, and a live chart shows the emotion counts.
4. **Play (60 s).** The game launches, and the goal is to collect as many orbs as possible. Emotions keep being recorded.
5. **Post-game (30 s).** A final round of recording after the game ends.
6. **Session report.** A donut chart of your quiz results, plus a bar chart for each phase, so you can see how your emotions changed.

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

The app opens the webcam and launches the game as a Windows executable, so it's meant to run on a Windows PC with a webcam.

**1. Install dependencies** (Python 3.10 recommended; the pinned TensorFlow and pandas versions need 3.8–3.10)

```bash
git clone https://github.com/arunekopsd/GISRv2.git
cd GISRv2
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
```

**2. Add the files that aren't in the repo**

The trained model and the game build are too large for GitHub, so they're excluded. Place them like this:

```
model/emotiondetector.json   model architecture
model/emotiondetector.h5     model weights
game/StackOBot.exe           game build (plus its data files)
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
