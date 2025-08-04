# Wordiviri

A fun and addictive 5-letter word guessing game built with SvelteKit and TypeScript. Test your vocabulary skills with this modern take on the classic word puzzle game!

## 🎮 Game Features

- **Word Guessing**: Guess 5-letter words in 6 attempts or less
- **Visual Feedback**: Color-coded tiles show your progress
  - 🟩 Green: Correct letter in correct position
  - 🟨 Yellow: Correct letter in wrong position
  - ⬜ Gray: Letter not in the word
- **Streak Tracking**: Keep track of your wins and losses
- **Auto-Continue**: Automatically starts a new round after each game
- **Persistent Storage**: Game state and streaks saved locally
- **Responsive Design**: Works on desktop and mobile devices
- **Smooth Animations**: Engaging visual effects and fireworks on wins

## 🚀 Getting Started

### Prerequisites

- Node.js 18+ 
- npm or yarn

### Installation

1. Clone the repository:
```bash
git clone <your-repo-url>
cd wordiviri
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

4. Open your browser and navigate to `http://localhost:5173`

## 🛠️ Development

### Available Scripts

- `npm run dev` - Start development server
- `npm run build` - Build for production
- `npm run preview` - Preview production build
- `npm run check` - Run TypeScript type checking
- `npm run check:watch` - Run type checking in watch mode

### Project Structure

```
src/
├── routes/
│   ├── +layout.svelte    # Global layout
│   └── +page.svelte      # Main game component
├── lib/                  # Shared components/utilities
├── app.html             # HTML template
└── app.d.ts             # TypeScript declarations
```

## 🚀 Deployment to Vercel

This project is configured for easy deployment to Vercel:

### Method 1: Vercel CLI

1. Install Vercel CLI:
```bash
npm i -g vercel
```

2. Deploy:
```bash
vercel
```

### Method 2: Git Integration

1. Push your code to GitHub/GitLab/Bitbucket
2. Connect your repository to Vercel
3. Vercel will automatically deploy on every push

### Method 3: Manual Deploy

1. Build the project:
```bash
npm run build
```

2. Upload the `.svelte-kit/output` directory to your hosting provider

## 🎯 How to Play

1. **Start Guessing**: Type a 5-letter word and press Enter
2. **Check Feedback**: Look at the color-coded tiles for hints
3. **Keep Guessing**: You have 6 attempts to find the word
4. **Win or Lose**: The game automatically starts a new round
5. **Track Progress**: Watch your win/loss streaks grow

## ⚡ Technologies Used

- **SvelteKit** - Full-stack framework
- **TypeScript** - Type safety and better development experience
- **Tailwind CSS** - Utility-first styling (via CDN)
- **Vite** - Fast build tool and development server
- **Vercel** - Deployment platform

## 🎨 Features

### Game Mechanics
- 50+ carefully selected 5-letter words
- Smart word validation
- Automatic game progression
- Local storage persistence

### UI/UX
- Clean, modern interface
- Smooth animations and transitions
- Responsive keyboard layout
- Visual feedback for every action
- Fireworks animation on wins

### Technical
- TypeScript for type safety
- Svelte stores for state management
- Component-based architecture
- Optimized for performance

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes and commit: `git commit -m 'Add feature'`
4. Push to the branch: `git push origin feature-name`
5. Submit a pull request

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 🎉 Enjoy Playing!

Challenge yourself and see how high you can get your win streak! 🏆
