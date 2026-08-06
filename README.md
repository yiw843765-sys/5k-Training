<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>✨ My 5k & BCT Glow Up Tracker ✨</title>
  <link href="https://fonts.googleapis.com/css2?family=Fredoka:wght@400;600&family=Quicksand:wght@500;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg-color: #FFF5F7;
      --card-bg: #FFFFFF;
      --primary-pink: #FF85A1;
      --soft-pink: #FFC2D1;
      --accent-purple: #B5179E;
      --text-dark: #4A4E69;
      --text-soft: #9A8C98;
      --mint: #70E4C8;
    }

    body {
      font-family: 'Quicksand', sans-serif;
      background-color: var(--bg-color);
      color: var(--text-dark);
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
    }

    .container {
      width: 100%;
      max-width: 450px;
      background: var(--card-bg);
      border-radius: 30px;
      padding: 25px;
      box-shadow: 0 10px 30px rgba(255, 133, 161, 0.15);
      border: 3px solid var(--soft-pink);
    }

    header {
      text-align: center;
      margin-bottom: 20px;
    }

    h1 {
      font-family: 'Fredoka', sans-serif;
      color: var(--primary-pink);
      font-size: 26px;
      margin: 5px 0;
    }

    .date-badge {
      display: inline-block;
      background: var(--soft-pink);
      color: var(--card-bg);
      font-weight: 700;
      padding: 6px 16px;
      border-radius: 20px;
      font-size: 14px;
      margin-top: 5px;
    }

    .workout-card {
      background: #FFF0F3;
      border-radius: 20px;
      padding: 20px;
      border: 2px dashed var(--primary-pink);
      margin-top: 15px;
    }

    .workout-title {
      font-family: 'Fredoka', sans-serif;
      color: var(--accent-purple);
      font-size: 20px;
      margin-bottom: 15px;
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .task-item {
      display: flex;
      align-items: center;
      background: var(--card-bg);
      padding: 12px 15px;
      border-radius: 15px;
      margin-bottom: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.03);
      gap: 12px;
      transition: all 0.2s ease;
    }

    .task-item input[type="checkbox"] {
      width: 20px;
      height: 20px;
      accent-color: var(--primary-pink);
      cursor: pointer;
    }

    .task-item label {
      font-size: 15px;
      font-weight: 600;
      cursor: pointer;
    }

    .task-item input[type="checkbox"]:checked + label {
      text-decoration: line-through;
      color: var(--text-soft);
    }

    .quote-box {
      text-align: center;
      font-size: 13px;
      color: var(--text-soft);
      margin-top: 20px;
      font-style: italic;
    }
  </style>
</head>
<body>

<div class="container">
  <header>
    <h1>✨ Daily Glow Up Tracker ✨</h1>
    <div class="date-badge" id="current-day-label">Loading...</div>
  </header>

  <div class="workout-card">
    <div class="workout-title" id="workout-title">🎯 Today's Mission</div>
    <div id="taskList"></div>
  </div>

  <div class="quote-box">
    🌸 "One workout closer to crushin' the 5k and Army BCT!" 🌸
  </div>
</div>

<script>
  const schedule = {
    0: { // Sunday
      title: "🛌 Sunday Rest & Recovery",
      tasks: [
        "Full rest day — let your muscles rebuild!",
        "10-15 mins light stretching / foam rolling",
        "Hydrate with plenty of water & electrolytes",
        "Prep gear and mental focus for Week 2"
      ]
    },
    1: { // Monday
      title: "🏋️‍♀️ Monday: Heavy Upper Push & Strength",
      tasks: [
        "3 x 8 Dumbbell RDLs (20 lbs)",
        "3 x 10 Smith Machine Incline Push-ups",
        "3 x 10 Cable Seated Rows (17.5 lbs)",
        "3 x 1-min Planks"
      ]
    },
    2: { // Tuesday
      title: "👟 Tuesday: 2-Mile Aerobic Jog",
      tasks: [
        "5-min walk + dynamic stretches",
        "2.0-Mile Jog at easy conversational pace (~10:00/mi)",
        "5-min cool-down walk + hamstring stretches"
      ]
    },
    3: { // Wednesday
      title: "🧘‍♀️ Wednesday: Active Recovery & Mobility",
      tasks: [
        "20-30 min easy outdoor walk or light spin",
        "Hamstring & Calf stretches (30s per leg)",
        "Cat-Cow & Cobra stretches for back relief",
        "Doorway chest opening stretches"
      ]
    },
    4: { // Thursday
      title: "🔥 Thursday: Tempo Run + Power & Grip",
      tasks: [
        "2.0-Mile Tempo Run (~9:40-9:50/mi pace)",
        "4 x 40m Heavy Dumbbell Farmer's Carries (20-25 lb DBs)",
        "3 x 10 Cable Horizontal Pallof Presses per side",
        "3 x 8-10 Dumbbell Overhead Presses (10-12.5 lb DBs)"
      ]
    },
    5: { // Friday
      title: "⚡ Friday: Bodyweight Circuit & Core",
      tasks: [
        "3 Rounds: 10-12 Incline Push-ups",
        "3 Rounds: 12 Dumbbell Goblet Squats",
        "3 Rounds: 10 Inverted Rows / Cable Pulldowns",
        "3 Rounds: 15 Walking Lunges",
        "3 Rounds: 45-60 Second Forearm Plank"
      ]
    },
    6: { // Saturday
      title: "🏃‍♀️ Saturday: Long Aerobic Endurance Run",
      tasks: [
        "5-min warm-up walk",
        "2.5-Mile Run at smooth easy pace (~10:00-10:15/mi)",
        "Take 1-min walk breaks if needed",
        "Post-run hydration & leg stretching"
      ]
    }
  };

  const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
  const today = new Date();
  const dayOfWeek = today.getDay();

  document.getElementById('current-day-label').innerText = `${days[dayOfWeek]} Focus`;

  const todayData = schedule[dayOfWeek];
  document.getElementById('workout-title').innerText = todayData.title;

  const taskListContainer = document.getElementById('taskList');
  taskListContainer.innerHTML = '';

  todayData.tasks.forEach((task, index) => {
    const taskDiv = document.createElement('div');
    taskDiv.className = 'task-item';
    
    const checkbox = document.createElement('input');
    checkbox.type = 'checkbox';
    checkbox.id = `task-${index}`;

    const label = document.createElement('label');
    label.htmlFor = `task-${index}`;
    label.innerText = task;

    taskDiv.appendChild(checkbox);
    taskDiv.appendChild(label);
    taskListContainer.appendChild(taskDiv);
  });
</script>

</body>
</html>
