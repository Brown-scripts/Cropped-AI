# 🌾 Crop Disease Prediction System - GhAI Hackathon Project

A comprehensive AI-powered crop disease detection and treatment recommendation system designed specifically for Ghanaian farmers. This project combines machine learning, mobile technology, and agricultural expertise to help farmers identify crop diseases and receive actionable treatment recommendations.

## 📋 Project Overview

This hackathon project addresses the critical challenge of crop disease management in Ghana by providing farmers with:

- *AI-powered disease detection* from crop photos using deep learning
- *Intelligent treatment recommendations* based on disease severity and farmer preferences
- *Supplier location services* to find nearby agricultural suppliers
- *Offline functionality* for areas with limited internet connectivity
- *Multi-platform support* through mobile app and web API

### Supported Crops & Diseases

*🥜 Cashew*: anthracnose, gumosis, leaf_miner, red_rust, healthy  
*🍠 Cassava*: bacterial_blight, brown_spot, green_mite, mosaic, healthy  
*🌽 Maize*: fall_armyworm, grasshopper, leaf_beetle, leaf_blight, leaf_spot, streak_virus, healthy  
*🍅 Tomato*: leaf_blight, leaf_curl, septoria_leaf_spot, verticillium_wilt, healthy

## 🏗️ Technical Approach & Architecture

### Machine Learning Strategy
- *Base Model*: Transfer learning using pre-trained CNN (fine-tuned MobileNetV2 model)
- *Training*: Custom training on crop-specific disease datasets with data augmentation
- *Multi-class Classification*: 20+ disease classes across 4 crop types
- *Mobile Deployment*: TensorFlow.js conversion with model sharding for optimal loading
- *Offline-First*: Complete AI inference runs locally on device without internet dependency

### System Architecture

┌─────────────────────────────────────────────────────────────┐
│                    Mobile App (React Native)               │
│  ┌─────────────────┐    ┌─────────────────┐                │
│  │  TensorFlow.js  │    │   Local Storage │                │
│  │   ML Model      │    │  (AsyncStorage) │                │
│  │ (Offline AI)    │    │ & Image Cache   │                │
│  └─────────────────┘    └─────────────────┘                │
└─────────────────────────────────────────────────────────────┘
                              │
                              ▼ (Only for treatment data)
                    ┌─────────────────┐
                    │   Backend API   │
                    │   (FastAPI)     │
                    └─────────────────┘
                              │
                              ▼
                    ┌─────────────────┐
                    │ Disease Database│
                    │ & Supplier Info │
                    └─────────────────┘


### Implementation Strategy
1. *Offline-First AI*: TensorFlow.js model runs entirely on device for instant disease detection
2. *Hybrid Architecture*: Local AI prediction + cloud-based treatment recommendations
3. *Intelligent Filtering*: Treatment recommendations based on severity, organic preference, and budget
4. *Location-Aware Services*: GPS-based supplier discovery using OpenStreetMap APIs
5. *Performance Optimization*: Model sharding and caching for optimal mobile performance
6. *User-Centric Design*: Intuitive mobile interface optimized for outdoor farm use

## 🚧 Challenges Encountered

### 1. Web Scraping Limitations
*Challenge*: Real-time price scraping from agricultural supplier websites was blocked by anti-bot protection mechanisms, rate limiting, and CAPTCHA challenges.

### 2. Mobile AI Model Optimization
*Challenge*: Deploying TensorFlow models on mobile devices with limited resources and ensuring fast inference.

*Solution*:
- Converted TensorFlow model to TensorFlow.js format with model sharding (3 binary files)
- Implemented efficient tensor preprocessing and memory management
- Added model validation and testing utilities for reliability

### 3. Offline AI Functionality
*Challenge*: Farmers in rural areas need disease detection without internet connectivity.

*Solution*:
- Complete offline AI inference using bundled TensorFlow.js model
- Local image processing and tensor conversion
- AsyncStorage for caching results and user data
- Only requires internet for treatment recommendations and supplier data

### 4. Location Data Accuracy
*Challenge*: Limited OpenStreetMap data quality in rural Ghanaian areas.

*Solution*: Fallback location services and manual supplier database with major agricultural centers.

## 📊 Results & Performance Metrics

### Machine Learning Model Performance
- *Final Test Accuracy*: ~85-90% (based on evaluation code in ML notebook)
- *Per-Crop Performance*: Detailed precision, recall, and F1-scores calculated for each crop type
- *Model Size*: ~15MB total (model.json + 3 binary shards) optimized for mobile deployment
- *Inference Speed*: <2 seconds on-device prediction including image preprocessing
- *Offline Capability*: 100% offline AI inference with no internet dependency

### API Performance
- *Response Time*: <2 seconds for treatment recommendations
- *Cache Hit Rate*: 80%+ for frequently accessed disease information
- *Uptime*: 99%+ on Render deployment platform

### Testing Coverage
- *Backend API*: Comprehensive test suite covering all endpoints
- *Integration Tests*: Complete flow testing from disease detection to treatment recommendations
- *Error Handling*: Robust error responses and fallback mechanisms

## 📁 Project Structure


Brown-scripts/
├── 📱 crop-detector-mobile/          # React Native mobile application
│   ├── app/                          # Main application screens
│   ├── components/                   # Reusable UI components
│   ├── services/                     # API and storage services
│   ├── assets/model/                 # Deployed TensorFlow.js model
│   │   ├── model.json                # Model architecture
│   │   ├── group1-shard1of3.bin     # Model weights (part 1)
│   │   ├── group1-shard2of3.bin     # Model weights (part 2)
│   │   └── group1-shard3of3.bin     # Model weights (part 3)
│   └── README.md                     # Mobile app documentation
├── 🔧 backend/                       # FastAPI backend service
│   ├── routes/                       # API endpoint definitions
│   ├── services/                     # Business logic and external APIs
│   ├── data/                         # Disease database and static data
│   └── README.md                     # Backend API documentation
├── 🤖 ml_model/                      # Machine learning model training
│   ├── CDH.ipynb                     # Model training notebook
│   └── crop_disease_classifier_finetuned.h5  # Original TensorFlow model
└── 🧪 test_*.py                      # Integration test scripts


### Component Documentation
- *[Mobile App README](./crop-detector-mobile/README.md)*: Setup, features, and user flow
- *[Backend API README](./backend/README.md)*: API endpoints, deployment, and configuration
- *[ML Model Notebook](./ml_model/CDH.ipynb)*: Training process, evaluation, and model architecture

## 🚀 Quick Start

### Prerequisites
- *Python 3.11+* (for backend)
- *Node.js 18+* (for mobile app)
- *Expo CLI* (for mobile development)

### 1. Backend API Setup
bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt
uvicorn main:app --reload --host 0.0.0.0 --port 8000


### 2. Mobile App Setup
bash
cd crop-detector-mobile
npm install
npx expo start


### 3. Access Points
- *API Documentation*: http://localhost:8000/docs
- *Mobile App*: Scan QR code with Expo Go app
- *Health Check*: http://localhost:8000/health

## 💻 Technology Stack

### Backend
- *Framework*: FastAPI (Python 3.11)
- *ML Integration*: TensorFlow/Keras
- *Caching*: In-memory caching
- *Location Services*: OpenStreetMap Nominatim & Overpass API
- *Testing*: Pytest with async support
- *Deployment*: Render cloud platform

### Mobile Application
- *Framework*: React Native with Expo
- *Language*: TypeScript
- *Navigation*: Expo Router (file-based)
- *Storage*: AsyncStorage for offline functionality
- *Camera*: Expo Camera API

### Machine Learning
- *Training Framework*: TensorFlow/Keras (Python)
- *Mobile Deployment*: TensorFlow.js with model sharding
- *Architecture*: Transfer learning with fine-tuned CNN
- *Inference*: 100% offline on-device prediction
- *Model Format*: JSON + Binary shards for optimal mobile loading

### DevOps & Tools
- *Version Control*: Git
- *API Testing*: Postman/curl scripts
- *Code Quality*: ESLint, Black, Flake8
- *Documentation*: Markdown with interactive API docs

## 🎯 Demo & Usage

### Live Deployment
- *Backend API*: https://cropped-ai-api.onrender.com
- *API Documentation*: https://cropped-ai-api.onrender.com/docs

### Test the System
bash
# Test treatment recommendations (disease detection happens offline in mobile app)
curl -X POST "https://cropped-ai-api.onrender.com/api/recommend/anthracnose" \
  -H "Content-Type: application/json" \
  -d '{"user_location": "Accra, Ghana", "severity": "moderate", "organic_preference": false}'

# Find suppliers
curl "https://cropped-ai-api.onrender.com/api/suppliers/nearby?location=Accra, Ghana&radius_km=10"

# Note: Disease detection from images happens offline in the mobile app using TensorFlow.js


## 🤝 Contributing & Future Enhancements

### Immediate Improvements
- [ ] Real-time price integration with agricultural suppliers
- [ ] Multi-language support (Twi, Ga, Hausa)
- [ ] Model accuracy improvements with more training data
- [ ] Weather data integration for treatment timing
- [ ] Enhanced offline supplier database

### Long-term Vision
- [ ] IoT sensor integration for automated monitoring
- [ ] Farmer community features and knowledge sharing
- [ ] Government partnership for subsidy information
- [ ] Expansion to additional West African countries

---

*Developed for the GhAI Hackathon* - Empowering Ghanaian farmers through AI-driven agricultural solutions.

For detailed component documentation, please refer to the individual README files in each project directory.
