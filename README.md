# GAME_PROGRAM-EX-2
## Create a player movement using character, collectable, player health and score

# Aim
Create a playable third-person character in Unreal Engine that can move and run, collect coin-like collectibles, track Score and Player Health, and display both on-screen (UI).

# Overview
1. Player Character (BP_PlayerCharacter) — handles movement, input, health, and overlaps with collectables.
2. Collectable (BP_Collectable) — simple actor with collision that gives score (and optionally health) when overlapped.
3. UI (WBP_HUD) — UMG Widget showing Score and Health values.
4. GameMode / PlayerState — optional for persistent Score/HighScore across respawns.
5. Game Flow:
   ▪ Collecting items increments Score.
   ▪ Health decreases when damaged.
   ▪ Player dies or respawns when Health is less than or equal to zero.

# Step-by-step Implementation (Blueprint First)

## 1. Project & Input Setup
Create a Third Person Blueprint project (or use the existing ThirdPersonMap).

Project Settings → Input  
Add these mappings:

▪ MoveForward (W / Up Arrow)  
▪ MoveRight (A / D or Left / Right)  
▪ Turn / LookUp (Mouse)  
▪ Jump (SpaceBar)  
▪ Run (Left Shift) — optional

## 2. Player Character Blueprint (BP_PlayerCharacter)

### A. Create Blueprint
Duplicate ThirdPersonCharacter and rename it to BP_PlayerCharacter.

### B. Add Variables
▪ Score (Integer), default: 0  
▪ MaxHealth (Float), example: 100  
▪ Health (Float), default = MaxHealth  
▪ bIsRunning (Boolean)

### C. Movement Setup
Use Add Movement Input for MoveForward and MoveRight.  
Use Turn and LookUp for mouse rotation.

Sprint (optional):  
▪ On Run Pressed → Set Max Walk Speed = 1200  
▪ On Released → Reset = 600

### D. Health Functions

#### Function: ApplyDamage(float DamageAmount)
- Subtract DamageAmount from Health.  
- If Health <= 0, call OnDeath (disable input, play animation, respawn or show Game Over).  
- Update HUD (call health update event).

#### Function: AddHealth(float HealAmount)
- Add HealAmount to Health, clamp to MaxHealth.  
- Update HUD.

### E. Score Management

#### Function: AddScore(int Amount)
- Score = Score + Amount  
- Call HUD update function

## 3. Collectable Blueprint (BP_Collectable)

OnComponentBeginOverlap (Sphere):

1. Check Other Actor → Cast to BP_PlayerCharacter  
2. If cast is successful:  
   ▪ Call AddScore(ScoreValue)  
   ▪ If GiveHealth > 0, call AddHealth(GiveHealth)  
   ▪ Play sound  
   ▪ Spawn particle effect  
   ▪ Destroy Actor  

## 4. BP_PlayerCharacter Custom Events

### AddScore Event
- Input: Amount (int)  
- Score = Score + Amount  
- Call UpdateScoreDisplay  
- (Optional) Play sound or spawn floating text

### ApplyDamage Event
- Health -= Damage  
- If Health <= 0 → Call OnDeath  
- Call UpdateHealthDisplay

# Output (Screenshots)
<img width="650" height="265" alt="image" src="https://github.com/user-attachments/assets/89fbc5db-1eb2-4ad3-8864-04f196636f63" />
<img width="1026" height="654" alt="image" src="https://github.com/user-attachments/assets/8a8c2e10-8b2c-4a9a-a21f-4d9bdb2cfcc6" />
<img width="1026" height="654" alt="image" src="https://github.com/user-attachments/assets/3cab7f7e-aded-45dd-968b-b2cf9a585726" />
<img width="996" height="504" alt="image" src="https://github.com/user-attachments/assets/1f9ac896-f9d3-4cb0-8766-6e3422ec1a0a" />


# Result
The AI character successfully roams within the NavMesh area using Behavior Tree logic.  
The player can move, run, jump, collect items, gain score, lose health, and display both Score and Health on-screen using the UI.
