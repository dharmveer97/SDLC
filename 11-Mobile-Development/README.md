# 📱 Mobile Development - React Native & Expo

## 🎯 What Is Mobile Development?

Mobile development = Building apps for phones and tablets

### Types of Mobile Apps:

1. **Native Apps**
   - iOS: Swift/Objective-C (Xcode)
   - Android: Java/Kotlin (Android Studio)
   - Best performance, platform-specific

2. **Cross-Platform Apps**
   - React Native, Flutter, Xamarin
   - One codebase for both platforms
   - Good performance, shared code

3. **Web Apps (PWA)**
   - Web technologies (HTML, CSS, JS)
   - Run in browser
   - Limited device access

## ⚛️ React Native - Write Once, Run Everywhere

### What Is React Native?

React Native = React for mobile apps

Instead of:
- Learning Swift for iOS
- Learning Kotlin for Android
- Building two separate apps

You can:
- Use JavaScript/TypeScript
- Share code between platforms
- Still get native performance

### React Native vs React Web

```jsx
// React Web
import React from 'react';

function App() {
  return (
    <div className="container">
      <h1>Hello World</h1>
      <button onClick={handleClick}>Click me</button>
    </div>
  );
}

// React Native
import React from 'react';
import { View, Text, TouchableOpacity } from 'react-native';

function App() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello World</Text>
      <TouchableOpacity onPress={handleClick}>
        <Text>Click me</Text>
      </TouchableOpacity>
    </View>
  );
}
```

## 🚀 Expo - React Native Made Easy

### What Is Expo?

Expo = React Native with superpowers

Expo provides:
- Easy setup (no Xcode/Android Studio needed)
- Pre-built components
- Over-the-air updates
- Testing on real devices
- Publishing to app stores

### Setting Up Expo

```bash
# Install Expo CLI
npm install -g @expo/cli

# Create new project
npx create-expo-app MyApp
cd MyApp

# Start development server
npx expo start

# Install Expo Go app on your phone to test
```

### Basic Expo App Structure

```
MyApp/
├── App.js              # Main app component
├── app.json            # App configuration
├── package.json        # Dependencies
├── assets/             # Images, fonts
├── components/         # Reusable components
└── screens/            # App screens
```

## 📱 Core React Native Components

### 1. View - Like `<div>`

```jsx
import { View } from 'react-native';

function Container({ children }) {
  return (
    <View style={{
      flex: 1,
      backgroundColor: '#f5f5f5',
      padding: 20
    }}>
      {children}
    </View>
  );
}
```

### 2. Text - For all text

```jsx
import { Text } from 'react-native';

function Typography() {
  return (
    <>
      <Text style={{ fontSize: 24, fontWeight: 'bold' }}>
        Title
      </Text>
      <Text style={{ fontSize: 16, color: '#666' }}>
        Subtitle
      </Text>
    </>
  );
}
```

### 3. TouchableOpacity - Clickable elements

```jsx
import { TouchableOpacity, Text } from 'react-native';

function Button({ title, onPress }) {
  return (
    <TouchableOpacity
      style={{
        backgroundColor: '#007AFF',
        padding: 15,
        borderRadius: 8,
        alignItems: 'center'
      }}
      onPress={onPress}
    >
      <Text style={{ color: 'white', fontSize: 16, fontWeight: '600' }}>
        {title}
      </Text>
    </TouchableOpacity>
  );
}
```

### 4. ScrollView - Scrollable content

```jsx
import { ScrollView, View, Text } from 'react-native';

function LongList() {
  return (
    <ScrollView style={{ flex: 1 }}>
      {Array.from({ length: 50 }, (_, i) => (
        <View key={i} style={{ padding: 20, borderBottomWidth: 1 }}>
          <Text>Item {i + 1}</Text>
        </View>
      ))}
    </ScrollView>
  );
}
```

### 5. FlatList - Efficient lists

```jsx
import { FlatList, View, Text } from 'react-native';

function UserList({ users }) {
  const renderUser = ({ item }) => (
    <View style={{ padding: 15, borderBottomWidth: 1 }}>
      <Text style={{ fontSize: 18, fontWeight: 'bold' }}>
        {item.name}
      </Text>
      <Text style={{ color: '#666' }}>
        {item.email}
      </Text>
    </View>
  );

  return (
    <FlatList
      data={users}
      renderItem={renderUser}
      keyExtractor={(item) => item.id.toString()}
      refreshing={false}
      onRefresh={() => {/* Refresh logic */}}
    />
  );
}
```

## 🎨 Styling in React Native

### StyleSheet API

```jsx
import { StyleSheet, View, Text } from 'react-native';

function StyledComponent() {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Hello</Text>
      <Text style={[styles.text, styles.highlighted]}>World</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: 'center',
    alignItems: 'center',
    backgroundColor: '#f5f5f5'
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    color: '#333',
    marginBottom: 10
  },
  text: {
    fontSize: 16,
    color: '#666'
  },
  highlighted: {
    backgroundColor: 'yellow',
    padding: 5
  }
});
```

### Flexbox Layout

```jsx
// React Native uses Flexbox by default
const styles = StyleSheet.create({
  // Vertical layout (default)
  column: {
    flexDirection: 'column',
    justifyContent: 'space-between',
    alignItems: 'center'
  },
  
  // Horizontal layout
  row: {
    flexDirection: 'row',
    justifyContent: 'space-around',
    alignItems: 'center'
  },
  
  // Taking available space
  expanded: {
    flex: 1  // Takes all available space
  },
  
  // Fixed size
  fixed: {
    width: 100,
    height: 50
  }
});
```

## 🧭 Navigation

### React Navigation Setup

```bash
npm install @react-navigation/native
npx expo install react-native-screens react-native-safe-area-context
npm install @react-navigation/stack
```

### Stack Navigation

```jsx
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';

const Stack = createStackNavigator();

function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Home">
        <Stack.Screen 
          name="Home" 
          component={HomeScreen}
          options={{ title: 'Welcome' }}
        />
        <Stack.Screen 
          name="Profile" 
          component={ProfileScreen}
          options={{ title: 'My Profile' }}
        />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

function HomeScreen({ navigation }) {
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Home Screen</Text>
      <Button
        title="Go to Profile"
        onPress={() => navigation.navigate('Profile', { userId: 123 })}
      />
    </View>
  );
}

function ProfileScreen({ route, navigation }) {
  const { userId } = route.params;
  
  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Profile Screen for User {userId}</Text>
      <Button
        title="Go Back"
        onPress={() => navigation.goBack()}
      />
    </View>
  );
}
```

### Tab Navigation

```jsx
import { createBottomTabNavigator } from '@react-navigation/bottom-tabs';
import Icon from 'react-native-vector-icons/Ionicons';

const Tab = createBottomTabNavigator();

function TabNavigator() {
  return (
    <Tab.Navigator
      screenOptions={({ route }) => ({
        tabBarIcon: ({ focused, color, size }) => {
          let iconName;
          
          if (route.name === 'Home') {
            iconName = focused ? 'home' : 'home-outline';
          } else if (route.name === 'Search') {
            iconName = focused ? 'search' : 'search-outline';
          }
          
          return <Icon name={iconName} size={size} color={color} />;
        },
        tabBarActiveTintColor: '#007AFF',
        tabBarInactiveTintColor: 'gray',
      })}
    >
      <Tab.Screen name="Home" component={HomeStack} />
      <Tab.Screen name="Search" component={SearchScreen} />
      <Tab.Screen name="Profile" component={ProfileScreen} />
    </Tab.Navigator>
  );
}
```

## 📡 API Integration

### Fetching Data

```jsx
import { useState, useEffect } from 'react';
import { View, Text, FlatList, ActivityIndicator } from 'react-native';

function PostList() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    fetchPosts();
  }, []);

  const fetchPosts = async () => {
    try {
      setLoading(true);
      const response = await fetch('https://jsonplaceholder.typicode.com/posts');
      const data = await response.json();
      setPosts(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  if (loading) {
    return (
      <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
        <ActivityIndicator size="large" color="#007AFF" />
        <Text>Loading posts...</Text>
      </View>
    );
  }

  if (error) {
    return (
      <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
        <Text style={{ color: 'red' }}>Error: {error}</Text>
      </View>
    );
  }

  const renderPost = ({ item }) => (
    <View style={{ padding: 15, borderBottomWidth: 1, borderColor: '#eee' }}>
      <Text style={{ fontSize: 16, fontWeight: 'bold', marginBottom: 5 }}>
        {item.title}
      </Text>
      <Text style={{ color: '#666' }}>
        {item.body}
      </Text>
    </View>
  );

  return (
    <FlatList
      data={posts}
      renderItem={renderPost}
      keyExtractor={(item) => item.id.toString()}
      refreshing={loading}
      onRefresh={fetchPosts}
    />
  );
}
```

## 🏪 State Management

### Using Context API

```jsx
import { createContext, useContext, useReducer } from 'react';

// Create context
const AppContext = createContext();

// Action types
const ACTIONS = {
  LOGIN: 'LOGIN',
  LOGOUT: 'LOGOUT',
  SET_LOADING: 'SET_LOADING'
};

// Reducer
function appReducer(state, action) {
  switch (action.type) {
    case ACTIONS.LOGIN:
      return {
        ...state,
        user: action.payload,
        isAuthenticated: true
      };
    case ACTIONS.LOGOUT:
      return {
        ...state,
        user: null,
        isAuthenticated: false
      };
    case ACTIONS.SET_LOADING:
      return {
        ...state,
        loading: action.payload
      };
    default:
      return state;
  }
}

// Provider component
export function AppProvider({ children }) {
  const [state, dispatch] = useReducer(appReducer, {
    user: null,
    isAuthenticated: false,
    loading: false
  });

  const login = async (email, password) => {
    dispatch({ type: ACTIONS.SET_LOADING, payload: true });
    try {
      // API call
      const user = await loginAPI(email, password);
      dispatch({ type: ACTIONS.LOGIN, payload: user });
    } catch (error) {
      console.error('Login failed:', error);
    } finally {
      dispatch({ type: ACTIONS.SET_LOADING, payload: false });
    }
  };

  const logout = () => {
    dispatch({ type: ACTIONS.LOGOUT });
  };

  return (
    <AppContext.Provider value={{
      ...state,
      login,
      logout
    }}>
      {children}
    </AppContext.Provider>
  );
}

// Hook to use context
export function useApp() {
  const context = useContext(AppContext);
  if (!context) {
    throw new Error('useApp must be used within AppProvider');
  }
  return context;
}
```

## 📱 Device Features

### Camera Integration

```jsx
import { Camera } from 'expo-camera';
import { useState, useRef } from 'react';
import { View, Text, TouchableOpacity } from 'react-native';

function CameraScreen() {
  const [hasPermission, setHasPermission] = useState(null);
  const [type, setType] = useState(Camera.Constants.Type.back);
  const cameraRef = useRef(null);

  useEffect(() => {
    (async () => {
      const { status } = await Camera.requestCameraPermissionsAsync();
      setHasPermission(status === 'granted');
    })();
  }, []);

  const takePicture = async () => {
    if (cameraRef.current) {
      const photo = await cameraRef.current.takePictureAsync();
      console.log('Photo taken:', photo.uri);
      // Save or upload photo
    }
  };

  if (hasPermission === null) {
    return <View />;
  }
  if (hasPermission === false) {
    return <Text>No access to camera</Text>;
  }

  return (
    <View style={{ flex: 1 }}>
      <Camera 
        style={{ flex: 1 }} 
        type={type}
        ref={cameraRef}
      >
        <View style={{ flex: 1, backgroundColor: 'transparent', flexDirection: 'row' }}>
          <TouchableOpacity
            style={{ position: 'absolute', bottom: 20, left: 20 }}
            onPress={() => {
              setType(
                type === Camera.Constants.Type.back
                  ? Camera.Constants.Type.front
                  : Camera.Constants.Type.back
              );
            }}
          >
            <Text style={{ fontSize: 18, color: 'white' }}>Flip Camera</Text>
          </TouchableOpacity>
          
          <TouchableOpacity
            style={{ position: 'absolute', bottom: 20, right: 20 }}
            onPress={takePicture}
          >
            <Text style={{ fontSize: 18, color: 'white' }}>Take Photo</Text>
          </TouchableOpacity>
        </View>
      </Camera>
    </View>
  );
}
```

### Location Services

```jsx
import * as Location from 'expo-location';
import { useState, useEffect } from 'react';

function LocationScreen() {
  const [location, setLocation] = useState(null);
  const [errorMsg, setErrorMsg] = useState(null);

  useEffect(() => {
    getCurrentLocation();
  }, []);

  const getCurrentLocation = async () => {
    try {
      let { status } = await Location.requestForegroundPermissionsAsync();
      if (status !== 'granted') {
        setErrorMsg('Permission to access location was denied');
        return;
      }

      let location = await Location.getCurrentPositionAsync({});
      setLocation(location);
    } catch (error) {
      setErrorMsg('Error getting location');
    }
  };

  const getAddressFromCoords = async () => {
    if (location) {
      try {
        const addresses = await Location.reverseGeocodeAsync({
          latitude: location.coords.latitude,
          longitude: location.coords.longitude
        });
        
        if (addresses.length > 0) {
          const address = addresses[0];
          console.log('Address:', `${address.street}, ${address.city}`);
        }
      } catch (error) {
        console.error('Error getting address:', error);
      }
    }
  };

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      {errorMsg ? (
        <Text style={{ color: 'red' }}>{errorMsg}</Text>
      ) : location ? (
        <View>
          <Text>Latitude: {location.coords.latitude}</Text>
          <Text>Longitude: {location.coords.longitude}</Text>
          <TouchableOpacity 
            style={{ marginTop: 20 }}
            onPress={getAddressFromCoords}
          >
            <Text style={{ color: 'blue' }}>Get Address</Text>
          </TouchableOpacity>
        </View>
      ) : (
        <Text>Getting location...</Text>
      )}
    </View>
  );
}
```

### Push Notifications

```jsx
import * as Notifications from 'expo-notifications';
import { useEffect, useRef, useState } from 'react';

// Configure notifications
Notifications.setNotificationHandler({
  handleNotification: async () => ({
    shouldShowAlert: true,
    shouldPlaySound: false,
    shouldSetBadge: false,
  }),
});

function NotificationScreen() {
  const [expoPushToken, setExpoPushToken] = useState('');
  const notificationListener = useRef();
  const responseListener = useRef();

  useEffect(() => {
    registerForPushNotificationsAsync().then(token => setExpoPushToken(token));

    // Listen for notifications
    notificationListener.current = Notifications.addNotificationReceivedListener(notification => {
      console.log('Notification received:', notification);
    });

    responseListener.current = Notifications.addNotificationResponseReceivedListener(response => {
      console.log('Notification tapped:', response);
    });

    return () => {
      Notifications.removeNotificationSubscription(notificationListener.current);
      Notifications.removeNotificationSubscription(responseListener.current);
    };
  }, []);

  const sendPushNotification = async () => {
    await Notifications.scheduleNotificationAsync({
      content: {
        title: "You've got mail! 📬",
        body: 'Here is the notification body',
        data: { someData: 'goes here' },
      },
      trigger: { seconds: 2 },
    });
  };

  return (
    <View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
      <Text>Push token: {expoPushToken}</Text>
      <TouchableOpacity onPress={sendPushNotification}>
        <Text style={{ color: 'blue' }}>Send Notification</Text>
      </TouchableOpacity>
    </View>
  );
}
```

## 📦 Building a Complete App

### Todo App Example

```jsx
import React, { useState } from 'react';
import {
  View,
  Text,
  TextInput,
  TouchableOpacity,
  FlatList,
  StyleSheet,
  Alert
} from 'react-native';

function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [inputText, setInputText] = useState('');

  const addTodo = () => {
    if (inputText.trim()) {
      setTodos([
        ...todos,
        {
          id: Date.now(),
          text: inputText,
          completed: false
        }
      ]);
      setInputText('');
    }
  };

  const toggleTodo = (id) => {
    setTodos(todos.map(todo =>
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
  };

  const deleteTodo = (id) => {
    Alert.alert(
      'Delete Todo',
      'Are you sure you want to delete this todo?',
      [
        { text: 'Cancel', style: 'cancel' },
        { 
          text: 'Delete', 
          style: 'destructive',
          onPress: () => setTodos(todos.filter(todo => todo.id !== id))
        }
      ]
    );
  };

  const renderTodo = ({ item }) => (
    <View style={styles.todoItem}>
      <TouchableOpacity 
        style={styles.todoText}
        onPress={() => toggleTodo(item.id)}
      >
        <Text style={[
          styles.todoTextContent,
          item.completed && styles.completedTodo
        ]}>
          {item.text}
        </Text>
      </TouchableOpacity>
      <TouchableOpacity
        style={styles.deleteButton}
        onPress={() => deleteTodo(item.id)}
      >
        <Text style={styles.deleteText}>❌</Text>
      </TouchableOpacity>
    </View>
  );

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Todo App</Text>
      
      <View style={styles.inputContainer}>
        <TextInput
          style={styles.input}
          placeholder="Add a new todo..."
          value={inputText}
          onChangeText={setInputText}
          onSubmitEditing={addTodo}
        />
        <TouchableOpacity style={styles.addButton} onPress={addTodo}>
          <Text style={styles.addButtonText}>Add</Text>
        </TouchableOpacity>
      </View>

      <FlatList
        data={todos}
        renderItem={renderTodo}
        keyExtractor={(item) => item.id.toString()}
        style={styles.todoList}
      />

      <Text style={styles.footer}>
        {todos.filter(t => !t.completed).length} of {todos.length} remaining
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
    padding: 20,
    paddingTop: 60
  },
  title: {
    fontSize: 24,
    fontWeight: 'bold',
    textAlign: 'center',
    marginBottom: 30
  },
  inputContainer: {
    flexDirection: 'row',
    marginBottom: 20
  },
  input: {
    flex: 1,
    borderWidth: 1,
    borderColor: '#ddd',
    padding: 15,
    backgroundColor: 'white',
    borderRadius: 8,
    marginRight: 10
  },
  addButton: {
    backgroundColor: '#007AFF',
    padding: 15,
    borderRadius: 8,
    justifyContent: 'center'
  },
  addButtonText: {
    color: 'white',
    fontWeight: 'bold'
  },
  todoList: {
    flex: 1
  },
  todoItem: {
    flexDirection: 'row',
    backgroundColor: 'white',
    padding: 15,
    marginBottom: 10,
    borderRadius: 8,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 1 },
    shadowOpacity: 0.2,
    shadowRadius: 2,
    elevation: 2
  },
  todoText: {
    flex: 1
  },
  todoTextContent: {
    fontSize: 16
  },
  completedTodo: {
    textDecorationLine: 'line-through',
    color: '#999'
  },
  deleteButton: {
    justifyContent: 'center',
    paddingLeft: 15
  },
  deleteText: {
    fontSize: 18
  },
  footer: {
    textAlign: 'center',
    marginTop: 20,
    color: '#666'
  }
});

export default TodoApp;
```

## 🚀 Publishing Your App

### Expo Build

```bash
# Build for iOS
expo build:ios

# Build for Android
expo build:android

# Or use EAS Build (new system)
npm install -g @expo/eas-cli
eas build --platform ios
eas build --platform android
```

### App Store Submission

1. **iOS (App Store)**
   - Need Apple Developer account ($99/year)
   - Use Xcode or EAS Submit
   - Review process (1-7 days)

2. **Android (Google Play)**
   - Google Play Console account ($25 one-time)
   - Upload APK/AAB
   - Review process (few hours to days)

## 🎯 React Native vs Other Options

### React Native Pros
- Code sharing between platforms
- Large community
- Facebook/Meta backing
- Hot reloading
- Access to native APIs

### React Native Cons
- Not truly native
- Bridge overhead
- Platform-specific bugs
- Large app size

### Alternatives

#### Flutter (Google)
```dart
// Dart language
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(title: Text('Flutter App')),
    body: Center(child: Text('Hello World')),
  );
}
```

#### Native Development
```swift
// iOS Swift
class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        let label = UILabel()
        label.text = "Hello World"
        view.addSubview(label)
    }
}
```

## 🎯 What This Means for BAs

Understanding mobile development helps you:

1. **Platform Decisions** - Native vs cross-platform trade-offs
2. **Feature Feasibility** - What's possible on mobile
3. **Performance Expectations** - Mobile limitations
4. **User Experience** - Mobile-specific patterns
5. **Development Timeline** - Cross-platform saves time

## ✅ Key Takeaways

- React Native enables cross-platform development
- Expo simplifies React Native development
- Mobile apps have unique UI components
- Navigation is different from web
- Device features require permissions
- Mobile performance matters more
- App store approval is required for distribution

Next Module: [DevOps & Deployment →](../12-DevOps-Deployment/README.md)