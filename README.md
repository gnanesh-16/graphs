# 📊 Architecture Diagram

![profile-picgnanesh-gemini](https://github.com/user-attachments/assets/48632e30-3c6b-4135-aff2-ab66a3446013)

## Web to Mobile Conversion Flow

```mermaid
flowchart TB
    subgraph WEB["ORIGINAL WEB APP (Next.js + React)"]
        direction LR
        pages["pages/<br/>index.jsx<br/>about.jsx<br/>_app.jsx"]
        src["src/<br/>App.jsx<br/>helpers"]
        public["public/<br/>manifest<br/>robots.txt<br/>sitemap"]
        webtech["Technologies:<br/>Next.js Router<br/>localStorage<br/>Tailwind CSS"]
        pages ~~~ src ~~~ public
    end
    
    WEB -->|CONVERSION| MOBILE
    
    subgraph MOBILE["MOBILE APP (React Native + Expo)"]
        direction TB
        AppRoot["App.js (Root)<br/>NavigationContainer + TabNavigator"]
        
        subgraph CoreLayer["Core Architecture"]
            direction LR
            Contexts["Contexts<br/>• Theme<br/>• Data"]
            Screens["Screens<br/>• Library<br/>• Brain<br/>• Stats<br/>• System<br/>• About"]
            Components["Components<br/>• TabBarIcon"]
        end
        
        Storage["AsyncStorage (Native)<br/>gn_links | gn_logs | gn_brain"]
        mobiletech["Technologies:<br/>React Navigation<br/>AsyncStorage<br/>StyleSheet"]
        
        AppRoot --> CoreLayer
        CoreLayer --> Storage
    end
    
    style WEB fill:#1e293b,stroke:#00f749,stroke-width:2px,color:#fff
    style MOBILE fill:#1e293b,stroke:#00f749,stroke-width:2px,color:#fff
    style AppRoot fill:#0a0a0a,stroke:#00f749,color:#00f749
    style CoreLayer fill:#0f172a,stroke:#334155,color:#fff
    style Storage fill:#7c3aed,stroke:#a78bfa,color:#fff
```

## Component Mapping

```
WEB COMPONENT                    →    MOBILE COMPONENT
─────────────────────────────────────────────────────────────────

Next.js Page                     →    Screen Component
  pages/index.jsx                →    LibraryScreen.js
  pages/about.jsx                →    AboutScreen.js
  src/App.jsx (tools view)       →    BrainScreen.js
  src/App.jsx (stats view)       →    StatsScreen.js
  src/App.jsx (sys view)         →    SystemScreen.js

Next.js Router                   →    React Navigation
  useRouter()                    →    useNavigation()
  router.push('/about')          →    navigation.navigate('_about')

HTML/CSS Elements                →    React Native Components
  <div>                          →    <View>
  <span>, <p>, <h1>             →    <Text>
  <input>                        →    <TextInput>
  <button>                       →    <TouchableOpacity>
  className="..."                →    style={styles.xxx}

Browser APIs                     →    Expo/React Native APIs
  localStorage                   →    AsyncStorage
  window.location                →    Linking
  navigator.clipboard            →    expo-clipboard
  File download                  →    expo-file-system
  Share API                      →    expo-sharing
  
State Management                 →    Context API
  Local state in components      →    ThemeContext
  Props drilling                 →    DataContext
```

## Data Flow Architecture

```mermaid
flowchart TD
    User["👤 USER INTERACTION"]
    
    subgraph Screens["SCREEN COMPONENTS"]
        Library["📚 LibraryScreen"]
        Brain["🧠 BrainScreen"]
        Stats["📊 StatsScreen"]
        System["⚙️ SystemScreen"]
        About["ℹ️ AboutScreen"]
    end
    
    subgraph Contexts["CONTEXT PROVIDERS"]
        direction LR
        Theme["ThemeContext<br/>• theme<br/>• colors<br/>• toggleTheme()"]
        Data["DataContext<br/>• links<br/>• logs<br/>• brain<br/>• addLink()<br/>• updateLink()<br/>• deleteLink()<br/>• exportData()"]
    end
    
    subgraph Storage["ASYNCSTORAGE (Device Storage)"]
        direction LR
        Links["gn_links"]
        Logs["gn_logs"]
        BrainData["gn_brain"]
        ThemeData["gn_theme"]
    end
    
    User --> Screens
    Screens --> Contexts
    Contexts --> Storage
    
    style User fill:#00f749,stroke:#00f749,color:#000
    style Screens fill:#1e293b,stroke:#00f749,color:#fff
    style Contexts fill:#0f172a,stroke:#3b82f6,color:#fff
    style Storage fill:#7c3aed,stroke:#a78bfa,color:#fff
    style Theme fill:#1e40af,stroke:#3b82f6,color:#fff
    style Data fill:#1e40af,stroke:#3b82f6,color:#fff
```

## Navigation Structure

```mermaid
graph TD
    App["APP<br/>(NavigationContainer)"]
    
    TabNav["TabNavigator<br/>Bottom Tab Navigation"]
    
    Library["📚<br/>LibraryScreen<br/>_library"]
    Brain["🧠<br/>BrainScreen<br/>_brain"]
    Stats["📊<br/>StatsScreen<br/>_stats"]
    System["⚙️<br/>SystemScreen<br/>_system"]
    About["ℹ️<br/>AboutScreen<br/>_about"]
    
    App --> TabNav
    TabNav --> Library
    TabNav --> Brain
    TabNav --> Stats
    TabNav --> System
    TabNav --> About
    
    style App fill:#0a0a0a,stroke:#00f749,stroke-width:3px,color:#00f749
    style TabNav fill:#1e293b,stroke:#00f749,stroke-width:2px,color:#00f749
    style Library fill:#0f172a,stroke:#3b82f6,color:#fff
    style Brain fill:#0f172a,stroke:#3b82f6,color:#fff
    style Stats fill:#0f172a,stroke:#3b82f6,color:#fff
    style System fill:#0f172a,stroke:#3b82f6,color:#fff
    style About fill:#0f172a,stroke:#3b82f6,color:#fff
```

## Storage Schema

```
AsyncStorage Keys:
┌─────────────────────────────────────────────────────────────────┐
│                                                                   │
│  gn_links:                                                        │
│  [                                                                │
│    {                                                              │
│      id: "abc123",                                               │
│      url: "https://example.com",                                 │
│      title: "Example Site",                                      │
│      tags: ["tech", "news"],                                     │
│      pinned: false,                                              │
│      archived: false,                                            │
│      clicks: 5,                                                  │
│      created: "2025-01-01T00:00:00.000Z",                       │
│      clicked: "2025-01-02T00:00:00.000Z"                        │
│    }                                                             │
│  ]                                                               │
│                                                                   │
│  gn_logs:                                                         │
│  [                                                                │
│    {                                                              │
│      id: "log123",                                               │
│      timestamp: "2025-01-01T00:00:00.000Z",                     │
│      action: "ADD",                                              │
│      details: "Created: Example Site"                            │
│    }                                                             │
│  ]                                                               │
│                                                                   │
│  gn_brain:                                                        │
│  {                                                                │
│    messages: [                                                    │
│      {                                                            │
│        id: 1,                                                     │
│        role: "user",                                             │
│        content: "Hello",                                         │
│        timestamp: "2025-01-01T00:00:00.000Z"                    │
│      }                                                           │
│    ],                                                            │
│    sources: [                                                     │
│      {                                                            │
│        id: 1,                                                     │
│        url: "https://example.com",                               │
│        title: "Example",                                         │
│        timestamp: "2025-01-01T00:00:00.000Z"                    │
│      }                                                           │
│    ]                                                             │
│  }                                                               │
│                                                                   │
│  gn_theme: "dark" | "light"                                      │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

## Platform Support

```mermaid
flowchart TD
    Source["Source Code<br/>React Native<br/>📱 Single Codebase<br/>100% Shared"]
    
    iOS["🍎 iOS<br/>• iPhone<br/>• iPad<br/><br/>App Store"]
    Android["🤖 Android<br/>• Phones<br/>• Tablets<br/><br/>Play Store"]
    
    Source --> iOS
    Source --> Android
    
    style Source fill:#00f749,stroke:#00f749,stroke-width:3px,color:#000
    style iOS fill:#1e293b,stroke:#3b82f6,color:#fff
    style Android fill:#1e293b,stroke:#10b981,color:#fff
```

## Build & Deploy Pipeline

```mermaid
flowchart LR
    Dev["💻 Development<br/><br/>expo start<br/><br/>Test on<br/>Expo Go"]
    
    Build["🏗️ Production<br/><br/>eas build<br/><br/>Android:<br/>• APK<br/>• AAB<br/><br/>iOS:<br/>• IPA"]
    
    Deploy["🚀 Distribution<br/><br/>App Store<br/>Review &<br/>Publish<br/><br/>TestFlight<br/>Google Play<br/>Internal Testing"]
    
    Dev --> Build --> Deploy
    
    style Dev fill:#1e293b,stroke:#00f749,color:#00f749
    style Build fill:#0f172a,stroke:#3b82f6,color:#fff
    style Deploy fill:#7c3aed,stroke:#a78bfa,color:#fff
```

---

**Visual Reference Only** - See actual code in the `src/` directory.
