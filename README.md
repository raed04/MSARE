# 🏟️ MSARE – *مساري*
### *AR-Powered Smart Stadium Navigation System*

---

## 📑 Table of Contents
- [Overview](#-overview)
- [Objectives](#-objectives)
- [Tech Stack](#-planned-tech-stack)
- [Contributing](#-contributing)
  - [Quick Start for Contributors](#quick-start-for-contributors)
  - [Detailed Workflow](#detailed-workflow)

---

## 📖 Overview  

**MSARE (مساري)** is a smart stadium navigation system that uses **Augmented Reality (AR)** and **indoor positioning technology** to guide fans through every step of their journey — from finding parking to reaching their seat and back after the event.  

### Key Benefits:
✅ Reduce congestion and improve crowd management  
✅ Seamless navigation experience for sports fans  
✅ Real-time positioning with AR-based directions  
✅ Live parking availability and crowd density updates  

By combining **Flutter**, **Situm SDK**, and **Firebase**, the app will deliver real-time positioning, AR-based navigation, and live updates about parking availability and crowd density.

---

## 🎯 Objectives  

- 🚗 Make it easier for fans to locate available parking near their seats  
- 🧭 Provide clear indoor navigation using AR overlays  
- 👥 Help organizers monitor and manage crowd movement in real-time  
- 🚶 Reduce congestion at stadium entrances and exits  
- 🇸🇦 Deliver a modern, digital fan experience aligned with **Saudi Vision 2030**

---

## 🧩 Planned Tech Stack  

| Component | Technology | Purpose |
|------------|-------------|----------|
| **Frontend** | Flutter (Dart) | Cross-platform mobile app for iOS & Android |
| **Backend** | Firebase (Cloud Firestore, Auth, Storage) | Real-time database, authentication, and file management |
| **Indoor Mapping** | Situm SDK | Indoor positioning, wayfinding, and AR navigation |
| **AR Framework** | ARCore / ARKit | Display AR arrows and overlays in the stadium |
| **Cloud Functions** | Node.js | Future integration with IoT cameras or AI crowd analytics |
| **Design Tool** | Figma | UI/UX design and screen mockups |

---

## 🤝 Contributing

We welcome contributions! Choose the guide that works best for you:

### Quick Start for Contributors

**First Time Setup:**
```bash
# 1. Fork the repo on GitHub, then clone your fork
git clone https://github.com/YOUR_USERNAME/MSARE.git
cd MSARE

# 2. Add the original repo as upstream
git remote add upstream https://github.com/AbdulmohsenMSARE/MSARE.git
```

**Making Changes:**
```bash
# 3. Create a new branch
git checkout -b feature/your-feature-name

# 4. Make your changes, then stage and commit
git add .
git commit -m "Add: Brief description of your changes"

# 5. Push to your fork
git push origin feature/your-feature-name
```

**6. Create Pull Request:**  
Go to GitHub → Your fork → Click "**New Pull Request**" → Fill out the details → Submit!

---

### Detailed Workflow

<details>
<summary><b>📥 Initial Setup (First Time Only)</b></summary>

#### 1. Fork the Repository
- Click the "**Fork**" button at the top right of the [GitHub repository page](https://github.com/AbdulmohsenMSARE/MSARE)
- This creates a copy in your GitHub account

#### 2. Clone Your Fork
```bash
git clone https://github.com/YOUR_USERNAME/MSARE.git
cd MSARE
```

#### 3. Add Upstream Remote
```bash
git remote add upstream https://github.com/AbdulmohsenMSARE/MSARE.git
```

</details>

<details>
<summary><b>🔧 Making Changes</b></summary>

#### 1. Create a New Branch
Always create a new branch for your work:
```bash
git checkout -b feature/your-feature-name
# Examples:
# git checkout -b feature/add-ar-navigation
# git checkout -b fix/parking-display-bug
```

#### 2. Make Your Changes
- Write your code
- Test thoroughly
- Follow the project's coding standards

#### 3. Commit Your Changes
```bash
# Stage specific files
git add path/to/file

# Or stage all changes
git add .

# Commit with a clear message
git commit -m "Add: Brief description of your changes"
```

**Commit Message Examples:**
- `Add: AR navigation feature`
- `Fix: Parking availability display issue`
- `Update: README with new setup instructions`

#### 4. Push to Your Fork
```bash
git push origin feature/your-feature-name
```

</details>

<details>
<summary><b>📤 Creating a Pull Request</b></summary>

1. Go to your fork on GitHub
2. Click "**Compare & Pull Request**" button
3. Fill out the PR template:
   - **What changed:** Description of your changes
   - **Why:** What problem it solves
   - **Testing:** How you tested it
4. Click "**Create Pull Request**"
5. Wait for review from maintainers

</details>

<details>
<summary><b>🔄 Keeping Your Fork Updated</b></summary>

Stay synchronized with the main repository:

```bash
# Fetch latest changes
git fetch upstream

# Switch to main branch
git checkout main

# Merge upstream changes
git merge upstream/main

# Push updates to your fork
git push origin main
```

**Update Your Feature Branch:**
```bash
git checkout feature/your-feature-name
git merge main
```

</details>

---

## 📝 Code of Conduct

- Be respectful and inclusive
- Follow the project's coding standards
- Write clear commit messages
- Test your changes before submitting

---

## 📞 Contact

For questions or suggestions, please open an issue or reach out to the maintainers.

---

**Happy Contributing! 🚀**