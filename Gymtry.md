# Zx
Trytry
<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>IronAI Fitness</title>
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://unpkg.com/react@18/umd/react.production.min.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js" crossorigin></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
<style>
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-track { background: #111827; }
::-webkit-scrollbar-thumb { background: #374151; border-radius: 3px; }


.hide-scroll::-webkit-scrollbar { display: none; }
    .hide-scroll { -ms-overflow-style: none; scrollbar-width: none; }

    @keyframes pulse-glow {
        0%, 100% { box-shadow: 0 0 10px rgba(132, 204, 22, 0.2); }
        50% { box-shadow: 0 0 20px rgba(132, 204, 22, 0.6); }
    }
    .ai-avatar-glow { animation: pulse-glow 2s infinite; }

    @keyframes fade-in-up {
        from { opacity: 0; transform: translateY(10px); }
        to { opacity: 1; transform: translateY(0); }
    }
    .animate-up { animation: fade-in-up 0.3s ease-out forwards; }

    .typing-dot { animation: typing 1.4s infinite ease-in-out both; }
    .typing-dot:nth-child(1) { animation-delay: -0.32s; }
    .typing-dot:nth-child(2) { animation-delay: -0.16s; }
    
    @keyframes typing {
        0%, 80%, 100% { transform: scale(0); }
        40% { transform: scale(1); }
    }

    body {
        background-color: #0f172a;
        color: #f8fafc;
        font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
        -webkit-tap-highlight-color: transparent;
    }
    .pb-safe { padding-bottom: env(safe-area-inset-bottom); }
</style>
</head>
<body>
<div id="root"></div>


<script type="text/babel">
    const { useState, useEffect, useRef } = React;

    // --- Data ---

    const WORKOUTS = [
        {
            id: 1,
            title: "Dumbbell Bicep Curls",
            target: "Arms",
            duration: "10 min",
            level: "Beginner",
            thumbnail: "https://images.unsplash.com/photo-1581009146145-b5ef050c2e1e?auto=format&fit=crop&q=80&w=600",
            videoUrl: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerEscapes.mp4"
        },
        {
            id: 2,
            title: "Overhead Shoulder Press",
            target: "Shoulders",
            duration: "15 min",
            level: "Intermediate",
            thumbnail: "https://images.unsplash.com/photo-1541534741688-6078c6bfb5c5?auto=format&fit=crop&q=80&w=600",
            videoUrl: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerBlazes.mp4"
        },
        {
            id: 3,
            title: "Tricep Dips",
            target: "Arms",
            duration: "8 min",
            level: "Beginner",
            thumbnail: "https://images.unsplash.com/photo-1583454110551-21f2fa2afe61?auto=format&fit=crop&q=80&w=600",
            videoUrl: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerJoyrides.mp4"
        },
        {
            id: 4,
            title: "Lateral Raises",
            target: "Shoulders",
            duration: "12 min",
            level: "Advanced",
            thumbnail: "https://images.unsplash.com/photo-1534438327276-14e5300c3a48?auto=format&fit=crop&q=80&w=600",
            videoUrl: "http://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ForBiggerMeltdowns.mp4"
        }
    ];

    const RECIPES = [
        {
            id: 1,
            title: "Lean Chicken Power Bowl",
            calories: "450 kcal",
            protein: "45g",
            image: "https://images.unsplash.com/photo-1546069901-ba9599a7e63c?auto=format&fit=crop&q=80&w=600",
            ingredients: ["200g Grilled Chicken Breast", "1 cup Quinoa", "Avocado", "Black Beans"],
            prep: "Grill chicken with paprika. Mix quinoa with beans. Top with sliced avocado."
        },
        {
            id: 2,
            title: "Post-Workout Berry Shake",
            calories: "320 kcal",
            protein: "30g",
            image: "https://images.unsplash.com/photo-1577805947697-89e18249d767?auto=format&fit=crop&q=80&w=600",
            ingredients: ["1 Scoop Whey Protein", "1 Banana", "1 cup Almond Milk", "Mixed Berries"],
            prep: "Blend all ingredients until smooth. Drink immediately after training for recovery."
        },
        {
            id: 3,
            title: "Salmon & Asparagus",
            calories: "520 kcal",
            protein: "40g",
            image: "https://images.unsplash.com/photo-1467003909585-2f8a7270028d?auto=format&fit=crop&q=80&w=600",
            ingredients: ["180g Salmon Fillet", "Lemon Juice", "Garlic", "Asparagus Spears"],
            prep: "Bake salmon at 200°C for 15 mins with lemon and garlic. Steam asparagus."
        },
        {
            id: 4,
            title: "Greek Yogurt Parfait",
            calories: "280 kcal",
            protein: "22g",
            image: "https://images.unsplash.com/photo-1488477181946-6428a0291777?auto=format&fit=crop&q=80&w=600",
            ingredients: ["1 cup Greek Yogurt", "Honey", "Walnuts", "Chia Seeds"],
            prep: "Layer yogurt with honey and nuts. A perfect slow-digesting protein snack before bed."
        }
    ];

    const LESSONS = [
        {
            id: 1,
            category: "Anatomy",
            title: "Understanding the Deltoids",
            content: "The shoulder muscle (deltoid) has three heads: Anterior (front), Lateral (side), and Posterior (rear). For broad shoulders, focus on the Lateral head.",
            readTime: "3 min"
        },
        {
            id: 2,
            category: "Technique",
            title: "Perfect Back Form",
            content: "Think about pulling with your elbows, not your hands. This engages the lats rather than the biceps. Keep chest up and spine neutral.",
            readTime: "4 min"
        },
        {
            id: 3,
            category: "Nutrition",
            title: "Protein Timing",
            content: "While total daily protein matters most, consuming 20-30g of protein within 2 hours post-workout maximizes muscle synthesis.",
            readTime: "2 min"
        }
    ];

    // --- Logic ---

    const generateAIResponse = (input) => {
        const lowerInput = input.toLowerCase();
        if (lowerInput.includes("recipe") || lowerInput.includes("food") || lowerInput.includes("eat")) {
            return "Nutrition is 70% of the game! Check out the 'Nutrition' tab for high-protein recipes like the Lean Chicken Power Bowl.";
        }
        if (lowerInput.includes("arm") || lowerInput.includes("bicep")) {
            return "For huge arms, focus on the squeeze! Try the Dumbbell Curls in the Workouts tab (Page 1).";
        }
        if (lowerInput.includes("shoulder") || lowerInput.includes("delts")) {
            return "Broad shoulders need lateral raises. I've added a great instructional video in the Workouts tab.";
        }
        return "I'm here to help with training and nutrition! Ask me about workouts or what to eat for gains.";
    };

    // --- Components ---

    const Header = () => (
        <header className="fixed top-0 w-full bg-slate-900/95 backdrop-blur-md border-b border-slate-800 z-50 px-4 py-3 flex items-center justify-between">
            <div className="flex items-center gap-2">
                <div className="w-8 h-8 rounded-full bg-gradient-to-tr from-lime-400 to-cyan-500 flex items-center justify-center text-slate-900 font-bold">
                    <i className="fas fa-dumbbell"></i>
                </div>
                <h1 className="text-xl font-bold bg-clip-text text-transparent bg-gradient-to-r from-lime-400 to-cyan-400">
                    IronAI
                </h1>
            </div>
            <div className="w-8 h-8 rounded-full bg-slate-800 flex items-center justify-center border border-slate-700">
                <i className="fas fa-user text-xs text-slate-400"></i>
            </div>
        </header>
    );

    const NavButton = ({ icon, label, active, onClick }) => (
        <button 
            onClick={onClick}
            className={`flex flex-col items-center justify-center w-full py-3 transition-colors duration-200 ${active ? 'text-lime-400' : 'text-slate-500 hover:text-slate-300'}`}
        >
            <i className={`fas ${icon} text-lg mb-1`}></i>
            <span className="text-[10px] uppercase font-bold tracking-wide">{label}</span>
        </button>
    );

    // PAGE 1: WORKOUTS
    const Workouts = () => {
        const [selectedWorkout, setSelectedWorkout] = useState(null);

        return (
            <div className="pt-20 pb-24 px-4 min-h-screen animate-up">
                <h2 className="text-2xl font-bold text-white mb-6">Workout Studio</h2>
                <div className="grid grid-cols-1 gap-4">
                    {WORKOUTS.map(workout => (
                        <div 
                            key={workout.id} 
                            onClick={() => setSelectedWorkout(workout)}
                            className="bg-slate-800 rounded-2xl overflow-hidden border border-slate-700 shadow-lg active:scale-[0.98] transition-transform cursor-pointer"
                        >
                            <div className="relative aspect-video">
                                <img src={workout.thumbnail} alt={workout.title} className="w-full h-full object-cover opacity-80" />
                                <div className="absolute inset-0 flex items-center justify-center">
                                    <div className="w-12 h-12 rounded-full bg-lime-500/90 text-slate-900 flex items-center justify-center shadow-xl backdrop-blur-sm">
                                        <i className="fas fa-play ml-1"></i>
                                    </div>
                                </div>
                                <div className="absolute bottom-2 right-2 bg-black/70 px-2 py-1 rounded text-xs font-medium backdrop-blur-md">
                                    {workout.duration}
                                </div>
                            </div>
                            <div className="p-4">
                                <h3 className="font-bold text-lg leading-tight text-white">{workout.title}</h3>
                                <div className="flex items-center gap-3 mt-2">
                                    <span className="text-xs text-lime-400 font-bold bg-lime-400/10 px-2 py-1 rounded">
                                        {workout.target}
                                    </span>
                                    <span className="text-xs text-slate-400">{workout.level}</span>
                                </div>
                            </div>
                        </div>
                    ))}
                </div>
                {selectedWorkout && (
                    <div className="fixed inset-0 z-[100] bg-black flex items-center justify-center animate-up">
                        <div className="absolute top-4 right-4 z-10">
                            <button onClick={() => setSelectedWorkout(null)} className="w-10 h-10 rounded-full bg-slate-800/80 text-white flex items-center justify-center backdrop-blur">
                                <i className="fas fa-times"></i>
                            </button>
                        </div>
                        <div className="w-full h-full flex flex-col">
                            <div className="flex-1 bg-black flex items-center justify-center">
                                <video controls autoPlay className="w-full max-h-[60vh] object-contain" poster={selectedWorkout.thumbnail}>
                                    <source src={selectedWorkout.videoUrl} type="video/mp4" />
                                </video>
                            </div>
                            <div className="p-6 bg-slate-900 border-t border-slate-800">
                                <h2 className="text-2xl font-bold text-white mb-2">{selectedWorkout.title}</h2>
                                <button onClick={() => setSelectedWorkout(null)} className="mt-4 w-full py-3 bg-gradient-to-r from-lime-500 to-lime-600 text-slate-900 font-bold rounded-xl">
                                    Complete Workout
                                </button>
                            </div>
                        </div>
                    </div>
                )}
            </div>
        );
    };

    // PAGE 2: NUTRITION (NEW)
    const Nutrition = () => {
        const [expandedRecipe, setExpandedRecipe] = useState(null);

        return (
            <div className="pt-20 pb-24 px-4 min-h-screen animate-up">
                <h2 className="text-2xl font-bold text-white mb-6">High Protein Fuel</h2>
                <div className="grid gap-5">
                    {RECIPES.map(recipe => (
                        <div key={recipe.id} className="bg-slate-800 rounded-2xl overflow-hidden border border-slate-700 shadow-lg">
                            <div className="relative h-40">
                                <img src={recipe.image} className="w-full h-full object-cover" />
                                <div className="absolute bottom-0 left-0 w-full bg-gradient-to-t from-slate-900 to-transparent p-4 pt-10">
                                    <h3 className="text-xl font-bold text-white">{recipe.title}</h3>
                                </div>
                            </div>
                            <div className="p-4">
                                <div className="flex justify-between items-center mb-4">
                                    <div className="flex items-center gap-2">
                                        <i className="fas fa-fire text-orange-500 text-sm"></i>
                                        <span className="text-sm font-semibold text-slate-200">{recipe.calories}</span>
                                    </div>
                                    <div className="flex items-center gap-2">
                                        <i className="fas fa-drumstick-bite text-lime-500 text-sm"></i>
                                        <span className="text-sm font-semibold text-lime-400">{recipe.protein} Protein</span>
                                    </div>
                                </div>
                                
                                <button 
                                    onClick={() => setExpandedRecipe(expandedRecipe === recipe.id ? null : recipe.id)}
                                    className="w-full py-2 bg-slate-700 hover:bg-slate-600 rounded-lg text-sm font-medium transition-colors text-slate-200"
                                >
                                    {expandedRecipe === recipe.id ? 'Hide Recipe' : 'View Recipe'}
                                </button>

                                {expandedRecipe === recipe.id && (
                                    <div className="mt-4 pt-4 border-t border-slate-700 animate-up">
                                        <h4 className="text-xs font-bold text-slate-400 uppercase mb-2">Ingredients</h4>
                                        <ul className="list-disc list-inside text-sm text-slate-300 mb-4 space-y-1">
                                            {recipe.ingredients.map((ing, i) => <li key={i}>{ing}</li>)}
                                        </ul>
                                        <h4 className="text-xs font-bold text-slate-400 uppercase mb-2">Preparation</h4>
                                        <p className="text-sm text-slate-300 leading-relaxed">{recipe.prep}</p>
                                    </div>
                                )}
                            </div>
                        </div>
                    ))}
                </div>
            </div>
        );
    };

    // PAGE 3: AI CHAT
    const AIChat = () => {
        const [messages, setMessages] = useState([
            { id: 1, sender: 'ai', text: "Hey! Coach Bolt here. I can help with your workout plan or suggest high-protein meals. What's on your mind?" }
        ]);
        const [input, setInput] = useState("");
        const [isTyping, setIsTyping] = useState(false);
        const chatEndRef = useRef(null);

        useEffect(() => chatEndRef.current?.scrollIntoView({ behavior: "smooth" }), [messages, isTyping]);

        const handleSend = (e) => {
            e.preventDefault();
            if (!input.trim()) return;
            const userMsg = { id: Date.now(), sender: 'user', text: input };
            setMessages(prev => [...prev, userMsg]);
            setInput("");
            setIsTyping(true);
            setTimeout(() => {
                const aiMsg = { id: Date.now() + 1, sender: 'ai', text: generateAIResponse(userMsg.text) };
                setMessages(prev => [...prev, aiMsg]);
                setIsTyping(false);
            }, 1200);
        };

        return (
            <div className="flex flex-col h-full pt-16 pb-20 animate-up">
                <div className="flex-1 overflow-y-auto px-4 py-4 space-y-4">
                    {messages.map((msg) => (
                        <div key={msg.id} className={`flex ${msg.sender === 'user' ? 'justify-end' : 'justify-start'}`}>
                            {msg.sender === 'ai' && (
                                <div className="w-8 h-8 rounded-full bg-lime-500 mr-2 flex items-center justify-center ai-avatar-glow flex-shrink-0">
                                    <i className="fas fa-robot text-slate-900 text-xs"></i>
                                </div>
                            )}
                            <div className={`max-w-[75%] px-4 py-3 rounded-2xl text-sm leading-relaxed ${
                                msg.sender === 'user' 
                                ? 'bg-gradient-to-br from-cyan-600 to-blue-600 text-white rounded-tr-none' 
                                : 'bg-slate-800 text-slate-200 border border-slate-700 rounded-tl-none'
                            }`}>
                                {msg.text}
                            </div>
                        </div>
                    ))}
                    {isTyping && (
                        <div className="flex justify-start">
                            <div className="w-8 h-8 rounded-full bg-lime-500 mr-2 flex items-center justify-center flex-shrink-0">
                                <i className="fas fa-robot text-slate-900 text-xs"></i>
                            </div>
                            <div className="bg-slate-800 border border-slate-700 px-4 py-3 rounded-2xl rounded-tl-none flex items-center gap-1">
                                <div className="w-2 h-2 bg-slate-400 rounded-full typing-dot"></div>
                                <div className="w-2 h-2 bg-slate-400 rounded-full typing-dot"></div>
                                <div className="w-2 h-2 bg-slate-400 rounded-full typing-dot"></div>
                            </div>
                        </div>
                    )}
                    <div ref={chatEndRef} />
                </div>
                <div className="fixed bottom-[60px] left-0 w-full bg-slate-900 border-
