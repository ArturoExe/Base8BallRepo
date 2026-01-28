# Base8BallRepo


---

## Tasks Checklist

### 1) App Entry
**File:** `Magic8BallApp.swift`
- [ ] Create the `@main` app entry.
- [ ] Set `Magic8BallView()` as the root view.

---

### 2) Motion Service (Core Motion)
**File:** `MotionService.swift`

**Goal:** read accelerometer values continuously and send them to the ViewModel.

#### Required Properties
- [ ] `private let motionManager: CMMotionManager`

#### Required Functions
- [ ] `func startAccelerometer(updateInterval:onUpdate:)`
  - Takes:
    - `updateInterval: TimeInterval`
    - `onUpdate: @escaping (Double, Double, Double) -> Void`
  - Must:
    - check `isAccelerometerAvailable`
    - set `accelerometerUpdateInterval`
    - call `startAccelerometerUpdates(to:)`
    - send `x, y, z` back through `onUpdate`
    
- [ ] `func stop()`
  - Must call `stopAccelerometerUpdates()`

---

### 3) ViewModel (Shake Logic + State)
**File:** `Magic8BallViewModel.swift`

**Goal:** store all UI state and implement shake detection.

#### Required @Published Properties
- [ ] `accelX`, `accelY`, `accelZ` (Double)
- [ ] `magnitude` (Double)
- [ ] `answerText` (String)
- [ ] `shakeCount` (Int)
- [ ] `isRunning` (Bool)

#### Required Non-Published Properties
- [ ] `private let motionService: MotionService`
- [ ] `private let answers: [String]`
- [ ] `private let shakeThreshold: Double`
- [ ] `private let cooldownSeconds: Double`
- [ ] `private var lastShakeTime: Date?`

#### Required Functions
- [ ] `func start()`
  - Prevent double-start
  - Start accelerometer updates
  - Update accel values + magnitude
  - Call shake detection
- [ ] `func stop()`
  - Stop accelerometer updates
  - Reset running state
- [ ] `private func calculateMagnitude(x:y:z:) -> Double`
  - `sqrt(x*x + y*y + z*z)`
- [ ] `private func detectShakeIfNeeded(currentMagnitude:)`
  - Check threshold
  - Check cooldown
  - Call `handleShake()` when valid
- [ ] `private func handleShake()`
  - Pick random answer
  - Increment `shakeCount` to trigger animation

---

### 4) View (UI + Animation)
**File:** `Magic8BallView.swift`

**Goal:** build the UI and animate when `shakeCount` changes.

#### Required UI Elements
- [ ] Title: `"Magic 8 Ball"`
- [ ] Ball UI (circle + “8”)
- [ ] Answer label showing `viewModel.answerText`
- [ ] Debug label: `Magnitude: ...` (recommended)
- [ ] Start / Stop buttons calling `viewModel.start()` and `viewModel.stop()`

#### Extra -> Shake Animation
- [ ] Create a `ShakeEffect: GeometryEffect`
  - Must use `animatableData`
  - Must move left/right using `sin(...)`
- [ ] Apply shake effect to the ball:
  - `.modifier(ShakeEffect(animatableData: ...))`
  - `.animation(..., value: viewModel.shakeCount)`

#### Lifecycle
- [ ] Start motion updates on appear (optional but recommended)
- [ ] Stop motion updates on disappear (required)

---

## Tuning Tips
- If it triggers too easily, increase `shakeThreshold` 
- If it barely triggers, decrease `shakeThreshold` 
- If it triggers many times per shake, increase `cooldownSeconds` 

---

## Definition of Done
- [ ] Shaking the phone triggers a **new random answer**
- [ ] The ball **animates** when a shake happens -> Extra 
- [ ] MVVM structure is respected (View → ViewModel → Service)
- [ ] Start/Stop works
- [ ] Motion updates stop when leaving the screen

---

## Extra Credit (Optional)
- [ ] Show `"Thinking..."` for 0.5 seconds before revealing the answer
- [ ] Add a subtle blur during the shake animation
- [ ] Add a “Shake again” hint text that fades in/out
