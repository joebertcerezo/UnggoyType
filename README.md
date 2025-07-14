# UnggoyType 🐒⌨️

A modern, responsive typing test application built with **Nuxt 3** and **Vue 3**. Practice your typing skills with customizable word counts and real-time statistics tracking.

## ✨ Features

- **Customizable Word Count**: Choose between 10, 25, or 50 words for your typing test
- **Real-time Statistics**: Track your WPM (Words Per Minute), accuracy, and character counts
- **Visual Feedback**:
  - Green highlighting for correct characters
  - Red highlighting for incorrect characters
  - Yellow cursor indicator with pulsing animation
- **Keyboard Support**: Full keyboard input handling with backspace support
- **Word Randomization**: Generate new random word sets without refreshing
- **Responsive Design**: Works seamlessly on desktop and mobile devices
- **Dark Theme**: Easy-on-the-eyes dark mode interface

## 🚀 Quick Start

### Prerequisites

- Node.js (v18 or higher)
- pnpm (recommended) or npm

### Installation

1. Clone the repository:

```bash
git clone <repository-url>
cd unggoy-type
```

2. Install dependencies:

```bash
pnpm install
# or
npm install
```

3. Start the development server:

```bash
pnpm dev
# or
npm run dev
```

4. Open your browser and navigate to `http://localhost:3000`

## 🎮 How to Use

1. **Select Word Count**: Click on 10, 25, or 50 to choose how many words you want to practice with
2. **Start Typing**: Simply start typing the words displayed on screen
3. **Track Progress**: Monitor your accuracy, WPM, and character counts in real-time
4. **Generate New Words**: Click "Generate New Words" to get a fresh set of random words
5. **Reset Test**: Click "Reset Test" to start over with the same words

## 🛠️ Tech Stack

- **Framework**: [Nuxt 3](https://nuxt.com/) - The Intuitive Vue Framework
- **Frontend**: [Vue 3](https://vuejs.org/) - The Progressive JavaScript Framework
- **Styling**: [TailwindCSS](https://tailwindcss.com/) - Utility-first CSS framework
- **Utilities**: [VueUse](https://vueuse.org/) - Collection of Vue composition utilities
- **Language**: TypeScript for type safety

## 📊 Statistics Tracked

- **WPM**: Words per minute calculation based on standard 5-character word length
- **Accuracy**: Percentage of correctly typed characters
- **Characters**: Real-time count of correct/incorrect/total characters typed

## 🎯 Keyboard Features

- **Full Alphabet Support**: All letters a-z
- **Special Characters**: Punctuation and symbols
- **Space Bar**: Proper word spacing
- **Backspace**: Correct mistakes as you type
- **Visual Feedback**: See which keys you're pressing (optional keyboard display)

## 🔧 Configuration

### Adding Custom Words

Edit the `constants/random-words.json` file to add your own word lists:

```json
{
  "words": ["your", "custom", "words", "here"]
}
```

### Customizing Word Counts

Modify the `wordCounts` array in `app.vue` to change available word count options:

```typescript
const wordCounts = [10, 25, 50, 100]; // Add more options
```

## 📁 Project Structure

```
unggoy-type/
├── app.vue              # Main application component
├── components/
│   └── Key.vue         # Virtual keyboard key component
├── constants/
│   ├── random-words.json # Word database
│   └── words.json      # Additional word sets
├── assets/
│   └── css/
│       └── main.css    # Global styles
├── nuxt.config.ts      # Nuxt configuration
├── package.json        # Dependencies and scripts
└── README.md          # This file
```

## 🚀 Available Scripts

- `pnpm dev` - Start development server
- `pnpm build` - Build for production
- `pnpm generate` - Generate static site
- `pnpm preview` - Preview production build

## 🎨 Customization

The application uses TailwindCSS for styling. You can easily customize:

- **Colors**: Modify the color scheme in the component classes
- **Typography**: Change font sizes, weights, and families
- **Layout**: Adjust spacing, positioning, and responsive breakpoints
- **Animations**: Customize the cursor pulse animation and transitions

## 📱 Responsive Design

UnggoyType is fully responsive and works on:

- Desktop computers
- Tablets
- Mobile phones
- Various screen sizes and orientations

## Screenshot


## 👨‍💻 Author

**Joebert Cerezo**

---

_Happy typing! 🎯_
