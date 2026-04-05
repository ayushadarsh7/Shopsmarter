# ShopSmarter: AI-Powered Personal Shopping Assistant for E-Commerce

<div align="center">
  <img src="https://img.shields.io/badge/Hackathon-ShopSmarter-blue" alt="Hackathon Badge">
  <img src="https://img.shields.io/badge/Team-AVTAR-red" alt="Team Badge">
  <img src="https://img.shields.io/badge/Python-3.11+-green" alt="Python Version">
  <img src="https://img.shields.io/badge/React-18+-blue" alt="React Version">
  <img src="https://img.shields.io/badge/Chrome%20Extension-Manifest%20V3-orange" alt="Chrome Extension">
  <img src="https://img.shields.io/badge/AI-Gemini%202.0-purple" alt="AI Model">
</div>

## Project Overview

**ShopSmarter** is an intelligent AI-powered Personal Shopping Assistant that revolutionizes the e-commerce experience by understanding visual inputs and automating the shopping process. Our system analyzes images of apparel, accessories, home decor, gadgets, and other products to suggest similar or complementary items, creating a truly personalized shopping journey.

###  Hackathon Problem Statement
*"Design and develop an AI-powered Personal Shopping Assistant that personalises the shopping experience for an e-commerce website and also automates the process. The system should understand visual inputs (such as apparel, accessories, home decor, gadgets, etc.) and suggest similar or complementary products available in the store."*

### Our Solution
ShopSmarter addresses this challenge through a comprehensive AI-driven platform that combines:
- **Advanced Computer Vision**: Multi-modal AI for understanding diverse product categories
- **Intelligent Recommendation Engine**: Context-aware suggestions for similar and complementary products
- **Automated Shopping Process**: Streamlined discovery-to-purchase workflow
- **Cross-Platform Integration**: Seamless experience across web and browser extension

##  Salient Features of Our Codebase

### **1. Multi-Modal AI Agent Architecture (`ai_agent.py`)**
Our core AI system leverages **LangGraph** with **Google Gemini 2.0 Flash** for sophisticated visual understanding:

```python
# Advanced state management with memory persistence
class State(TypedDict):
    messages: Annotated[list, add_messages]

# Multi-modal processing for images and text
llm = init_chat_model("gemini-2.0-flash", model_provider="google_genai")
llm_with_tools = llm.bind_tools([search_tool])
```

**Key Capabilities:**
- **Visual Product Analysis**: Identifies clothing styles, home decor themes, gadget specifications
- **Brand Recognition**: Detects and suggests similar branded products
- **Style Categorization**: Understands fashion aesthetics, interior design styles
- **Contextual Understanding**: Distinguishes between product categories (apparel vs. home decor vs. gadgets)

###  **2. Intelligent Search Integration (`tools.py`)**
Real-time product discovery through DuckDuckGo search integration:

```python
search_tool = Tool(
    name="DuckDuckGoSearch",
    func=search.run,
    description="A powerful tool for finding up-to-date product information and recommendations"
)
```

**Features:**
- Real-time e-commerce data fetching
- Product availability verification
- Price comparison across platforms
- Trend-aware recommendations

### **3. RESTful API Backend (`app.py`)**
Flask-powered backend supporting multi-modal interactions:

```python
@app.route('/chat', methods=['POST'])
def chat():
    user_message = request.form.get("message", "")
    image_file = request.files.get("image", None)
    
    # Multi-modal request processing
    user_request = [
        {"type": "text", "text": user_message},
        {"type": "image", "source_type": "base64", "data": image_data}
    ]
```

**Capabilities:**
- **Image Upload Processing**: Handles multiple image formats (JPEG, PNG, WebP)
- **Base64 Encoding**: Efficient image data transmission
- **Thread Management**: Persistent conversation history
- **CORS Support**: Cross-origin resource sharing for web integration

###  **4. Modern React Frontend (`ai_agent_interface/`)**
Responsive web application with rich user interactions:

```jsx
// Multi-modal message handling
const sendMessage = async () => {
    const formData = new FormData();
    if (trimmed) formData.append('message', trimmed);
    if (image) formData.append('image', image);
    
    // Real-time AI response processing
    const data = await res.json();
    setChatHistory(prev => [...prev, { role: 'assistant', content: replyContent }]);
};
```

**Features:**
- **Drag-and-Drop Image Upload**: Intuitive file handling
- **Real-time Chat Interface**: Instant AI responses
- **Markdown Support**: Rich text formatting for product links
- **Responsive Design**: Mobile-first approach
- **Image Preview**: Visual confirmation before sending

###  **5. Chrome Extension Integration (`ai_agent_extension/`)**
Seamless browser integration for in-page shopping assistance:

```javascript
// Advanced screen capture functionality
function captureSelectedArea(left, top, width, height) {
    chrome.runtime.sendMessage({
        action: 'captureTab',
        area: { left, top, width, height }
    });
}
```

**Advanced Features:**
- **Area Selection Tool**: Precise product capture from any website
- **Cross-tab Synchronization**: Persistent chat across browser tabs
- **Dynamic Sidebar**: Resizable and responsive UI
- **Background State Management**: Maintains shopping context

### **6. Intelligent Product Recommendation System**
Our AI system provides sophisticated product suggestions:

#### **Similar Product Discovery**
- Analyzes visual features (color, style, pattern, material)
- Matches products across different e-commerce platforms
- Considers price ranges and availability

#### **Complementary Product Suggestions**
- **Fashion**: Suggests matching accessories, shoes, bags for outfits
- **Home Decor**: Recommends coordinating furniture, lighting, textiles
- **Gadgets**: Proposes compatible accessories and complementary devices

#### **Category-Specific Intelligence**
```python
system_prompt = """
You are an intelligent Amazon Personal Shopping Assistant. Your core function is to analyze 
user-provided images and text queries to offer relevant product recommendations.

- **Fashion**: Offer similar items, style variations, or complementary accessories
- **Home**: Suggest lamps, curtains, rugs, or furniture that complement the style
- **Gadgets**: Recommend compatible accessories and related technology products
"""
```

### **7. Conversation Memory & State Management**
Persistent shopping context across sessions:

```python
# Memory-enabled conversation tracking
memory = MemorySaver()
graph = graph_builder.compile(checkpointer=memory)

def get_state(thread_id: str = "1") -> dict:
    config = {"configurable": {"thread_id": thread_id}}
    return graph.get_state(config=config)
```

**Benefits:**
- **Shopping History**: Maintains user preferences and past searches
- **Context Continuity**: Remembers previous product discussions
- **Cross-Session Persistence**: Resumes conversations seamlessly


**Features:**
- **Smart URL Generation**: Creates optimized Amazon search links
- **Product-Specific Queries**: Includes identified brands, styles, and specifications
- **Price-Aware Suggestions**: Considers budget ranges when available

## Architecture Overview

### System Flow
```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Visual Input  │    │   AI Analysis   │    │  Recommendations│
│                 │    │                 │    │                 │
│ • Image Upload  │───►│ • Gemini Vision │───►│ • Similar Items │
│ • Screen Capture│    │ • Style Analysis│    │ • Complementary │
│ • Product Photos│    │ • Brand ID      │    │ • Amazon Links  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Technology Stack Innovation

**AI & Machine Learning:**
- **Google Gemini 2.0 Flash**: State-of-the-art vision-language model
- **LangGraph**: Advanced AI agent orchestration
- **LangChain**: Tool integration and memory management
- **Multi-modal Processing**: Simultaneous text and image understanding

**Backend Architecture:**
- **Flask**: Lightweight, scalable REST API
- **Python 3.13**: Modern language features and performance
- **Base64 Encoding**: Efficient image data handling
- **CORS**: Cross-origin support for web integration

**Frontend Innovation:**
- **React 18+**: Modern component architecture
- **Vite**: Lightning-fast development and build
- **React Markdown**: Rich content rendering
- **Tailwind CSS**: Modern, responsive design system

**Browser Integration:**
- **Manifest V3**: Latest Chrome extension standards
- **Service Workers**: Background processing
- **Content Scripts**: Page interaction and capture
- **Screen Capture API**: Advanced area selection and image capture

##  Quick Start

### Prerequisites
- **Python 3.11+** 
- **Node.js 18+** and npm
- **Google API Key** (for Gemini AI)
- **Chrome Browser** (for extension)

### 1. Clone the Repository
```bash
git clone https://github.com/your-username/shopsmarter.git
cd ai_agent
```

### 2. Backend Setup
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create environment file
echo "GOOGLE_API_KEY=your_gemini_api_key_here" > .env

# Run the Flask backend
python app.py
```

### 3. Frontend Setup
```bash
cd ai_agent_interface

# Install dependencies
npm install

# Start development server
npm run dev

# For production build
npm run build && npm run preview
```

### 4. Chrome Extension Setup
1. Open Chrome and navigate to `chrome://extensions/`
2. Enable "Developer mode" (top right toggle)
3. Click "Load unpacked" and select the `ai_agent_extension/dist` folder
4. The ShopSmarter extension will appear in your browser toolbar

## Usage Guide

### Web Interface
1. **Visit**: `http://localhost:5173` (frontend) 
2. **Upload Image**: Drag & drop or click to upload product images
3. **Get Recommendations**: AI analyzes and suggests similar/complementary products
4. **Shop**: Click generated Amazon links to purchase

### Chrome Extension
1. **Screen Capture**: Click extension icon → "Capture Area"
2. **Select Product**: Draw selection box around any product on websites
3. **AI Analysis**: Extension automatically analyzes captured product
4. **Instant Results**: Get recommendations in the extension sidebar

### API Endpoints
- `POST /chat` - Send text/image for AI analysis
- `GET /state/<thread_id>` - Get conversation state
- Backend runs on `http://localhost:5000`
```

### Frontend Production Build
```bash
cd ai_agent_interface

# Build for production
npm run build

# Serve static files (dist folder)
npm run preview
```


##  Project Structure
```


appian_ai_agent-browser-tools/
├── ai_agent.py                 # Core AI agent implementation
├── app.py                      # Backend API and WebSocket handlers
├── tools.py                    # Browser tools and search functionality
├── system_prompt.py            # AI agent system prompt and capabilities
├── requirements.txt            # Project dependencies
├── README.md                   # Project documentation
│
├── tests/                      # Testing directory
│   ├── test_ai_agent.py       # Unit tests for AI agent
│   ├── benchmark_ai_agent.py  # Performance benchmarking
│   ├── BENCHMARK_STATS.md     # Benchmark results
│   └── PROJECT_STRUCTURE.md   # This file
│
├── ai_agent_extension/         # Chrome extension
│   ├── background.js          # Extension background script
│   ├── content.js             # Content script for page interaction
│   ├── popup.html             # Extension popup interface
│   │
│   ├── assets/               # Extension assets
│   │   ├── icon.jpeg        # Extension icon
│   │   └── remixicon.svg    # UI icons
│   │
│   ├── dist/                # Built extension files
│   │   ├── background.js    # Compiled background script
│   │   └── manifest.json    # Extension manifest
│   │
│   └── src/                 # Extension source code
│       └── popup/           # Popup interface components
│           ├── App.jsx      # Main popup component
│           ├── index.jsx    # Popup entry point
│           ├── components/  # UI components
│           ├── hooks/       # Custom React hooks
│           └── utils/       # Utility functions
│
├── ai_agent_interface/       # Web interface
│   ├── index.html           # Main interface
│   ├── App.jsx             # Main React component
│   ├── public/             # Static assets
│   └── src/                # Source code
│
└── images/                  # Project images and assets
```



## Key Features

###  **Modern UI/UX Design**
- **Glass Morphism Effects**: Beautiful modern interface with gradient backgrounds
- **Responsive Design**: Seamless experience across desktop, tablet, and mobile
- **Dark Theme**: Eye-friendly interface with sophisticated color schemes
- **Smooth Animations**: Fluid transitions and interactive elements

### **Advanced AI Capabilities**
- **Multi-Modal Understanding**: Processes both images and text simultaneously
- **Context Awareness**: Maintains conversation history and shopping preferences
- **Brand Recognition**: Identifies specific brands and suggests alternatives
- **Style Analysis**: Understands fashion trends, color coordination, and design aesthetics

###  **Smart Product Discovery**
- **Visual Search**: Upload any product image for instant recommendations
- **Screen Capture**: Capture products directly from any website
- **Category Intelligence**: Specialized recommendations for fashion, home decor, gadgets


## Contributing

We welcome contributions to **ShopSmarter**! Please follow these steps:

### Development Setup
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Set up the development environment:
   ```bash
   # Backend setup
   python -m venv venv
   source venv/bin/activate
   pip install -r requirements.txt
   
   # Frontend setup
   cd ai_agent_interface
   npm install
   npm run dev
   ```

### Making Changes
4. Make your changes following our coding standards:
   - **Python**: Follow PEP 8 style guidelines
   - **JavaScript/React**: Use ESLint configuration provided
   - **CSS**: Follow BEM methodology for class naming
   - **Git**: Use conventional commit messages

5. Test your changes:
   ```bash
   # Test backend
   python -m pytest  # (if tests exist)
   
   # Test frontend
   npm run lint
   npm run build
   
   # Test extension
   Load unpacked in Chrome and verify functionality
   ```

6. Commit your changes (`git commit -m 'feat: Add some AmazingFeature'`)
7. Push to the branch (`git push origin feature/AmazingFeature`)
8. Open a Pull Request with detailed description

### Areas for Contribution
- **UI/UX Enhancements**: Improve user interface and experience
- **Browser Compatibility**: Support for Firefox, Safari, Edge
- **Mobile App**: React Native implementation
- **Testing**: Add comprehensive test coverage
- **Documentation**: Improve guides and API documentation

## Troubleshooting

### Common Issues

**Backend Issues:**
```bash
# API key not working
echo "GOOGLE_API_KEY=your_actual_key" > .env

# Port already in use
lsof -ti:5000 | xargs kill -9
```

**Frontend Issues:**
```bash
# Dependencies issues
rm -rf node_modules package-lock.json
npm install

# Build failures
npm run build --verbose
```

**Extension Issues:**
1. Reload extension in Chrome after code changes
2. Check browser console for JavaScript errors
3. Verify manifest.json permissions


### Sample Use Cases
- **Fashion**: Upload outfit → Get matching accessories
- **Home Decor**: Capture furniture → Find complementary items  
- **Gadgets**: Show device → Discover compatible accessories
- **Shopping**: Screenshot product → Compare prices & alternatives

## Future Roadmap

### Phase 1 (Current)
- Multi-modal AI integration
- Chrome extension with screen capture
- Modern responsive web interface
- Real-time product recommendations

### Phase 2 (Upcoming)
- User accounts and shopping history
- Advanced filters and preferences
- Price tracking and alerts
- Mobile application (React Native)

### Phase 3 (Future)
- Multi-platform browser support
- AR/VR integration for virtual try-on
- Social shopping features
- Marketplace integration beyond Amazon



---

<div align="center">
  
  <br><br>
  
  <a href="#quick-start"> Get Started</a> •
  <a href="#contributing"> Contribute</a> •
  <a href="mailto:ayushadarsh2019@gmail.com"> Contact</a>
</div>
