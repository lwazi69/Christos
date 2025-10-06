# Christos

App.tsx
import React, { useState, useEffect } from 'react';
import { Image } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator, NativeStackScreenProps } from '@react-navigation/native-stack';
import * as Font from 'expo-font';
import AppLoading from 'expo-app-loading';

// Screens
import HomeScreen from './screens/Home';
import MenuScreen from './screens/Menu';
import AboutScreen from './screens/About';
import EnquiryScreen from './screens/Enquiry';
import GalleryScreen from './screens/Gallery';
import ReservationScreen from './screens/Reservation';

// TypeScript Navigation Types
export type RootStackParamList = {
  Home: undefined;
  Menu: undefined;
  About: undefined;
  Enquiry: undefined;
  Gallery: undefined;
  Reservation: undefined;
};

const Stack = createNativeStackNavigator<RootStackParamList>();

export default function App() {
  const [fontsLoaded, setFontsLoaded] = useState(false);

  useEffect(() => {
    Font.loadAsync({
      'playfair-display': require('./assets/fonts/PlayfairDisplay-Regular.ttf'),
      'playfair-bold': require('./assets/fonts/PlayfairDisplay-Bold.ttf'),
    }).then(() => setFontsLoaded(true));
  }, []);

  if (!fontsLoaded) return <AppLoading />;

  return (
    <NavigationContainer>
      <Stack.Navigator
        screenOptions={{
          headerStyle: { backgroundColor: '#000' },
          headerTintColor: '#FFD700',
          headerTitleStyle: { fontWeight: 'bold', fontFamily: 'playfair-bold' },
          headerRight: () => (
            <Image source={require('./assets/logo.png')} style={{ width: 40, height: 40, marginRight: 10 }} />
          ),
        }}
      >
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Menu" component={MenuScreen} />
        <Stack.Screen name="About" component={AboutScreen} />
        <Stack.Screen name="Enquiry" component={EnquiryScreen} />
        <Stack.Screen name="Gallery" component={GalleryScreen} />
        <Stack.Screen name="Reservation" component={ReservationScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

3️⃣ components/GoldButton.tsx
import React from 'react';
import { TouchableOpacity, Text, StyleSheet } from 'react-native';

interface GoldButtonProps {
  title: string;
  onPress: () => void;
}

export default function GoldButton({ title, onPress }: GoldButtonProps) {
  return (
    <TouchableOpacity style={styles.button} onPress={onPress}>
      <Text style={styles.text}>{title}</Text>
    </TouchableOpacity>
  );
}

const styles = StyleSheet.create({
  button: {
    backgroundColor: '#FFD700',
    paddingVertical: 12,
    paddingHorizontal: 25,
    borderRadius: 30,
    marginVertical: 5,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 6 },
    shadowOpacity: 0.4,
    shadowRadius: 8,
    elevation: 8,
  },
  text: {
    color: '#000',
    fontSize: 18,
    fontWeight: 'bold',
    textAlign: 'center',
    fontFamily: 'playfair-bold',
  },
});

4️⃣ screens/Home.tsx
import React from 'react';
import { View, Text, StyleSheet, ImageBackground } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { RootStackParamList } from '../App';
import GoldButton from '../components/GoldButton';

type Props = NativeStackScreenProps<RootStackParamList, 'Home'>;

export default function Home({ navigation }: Props) {
  return (
    <ImageBackground
      source={{ uri: 'https://images.unsplash.com/photo-1555992336-03a23f3f4e5c?fit=crop&w=1000&q=80' }}
      style={styles.background}
    >
      <View style={styles.container}>
        <Text style={styles.title}>Christophel’s Restaurant</Text>
        <Text style={styles.subtitle}>Fine Dining Experience</Text>

        <GoldButton title="View Menu" onPress={() => navigation.navigate('Menu')} />
        <GoldButton title="About Us" onPress={() => navigation.navigate('About')} />
        <GoldButton title="Gallery" onPress={() => navigation.navigate('Gallery')} />
        <GoldButton title="Enquiry" onPress={() => navigation.navigate('Enquiry')} />
        <GoldButton title="Reservation" onPress={() => navigation.navigate('Reservation')} />
      </View>
    </ImageBackground>
  );
}

const styles = StyleSheet.create({
  background: { flex: 1, resizeMode: 'cover' },
  container: { flex: 1, justifyContent: 'center', alignItems: 'center', backgroundColor: 'rgba(0,0,0,0.5)', padding: 20 },

screens/Menu.tsx
import React from 'react';
import { View, Text, StyleSheet, FlatList, Image } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { RootStackParamList } from '../App';

type Props = NativeStackScreenProps<RootStackParamList, 'Menu'>;

interface MenuItem {
  id: string;
  name: string;
  price: string;
  image: string;
}

const menuItems: MenuItem[] = [
  { id: '1', name: 'Grilled Salmon', price: '$25', image: 'https://images.unsplash.com/photo-1600891964599-f61ba0e24092?fit=crop&w=800&q=80' },
  { id: '2', name: 'Beef Steak', price: '$30', image: 'https://images.unsplash.com/photo-1600891964599-72d5b6a8b6d2?fit=crop&w=800&q=80' },
  { id: '3', name: 'Caesar Salad', price: '$15', image: 'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?fit=crop&w=800&q=80' },
];

export default function Menu({}: Props) {
  return (
    <View style={styles.container}>
      <FlatList
        data={menuItems}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.item}>
            <Image source={{ uri: item.image }} style={styles.image} />
            <Text style={styles.name}>{item.name}</Text>
            <Text style={styles.price}>{item.price}</Text>
          </View>
        )}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#000', padding: 10 },
  item: { marginBottom: 20, alignItems: 'center', borderBottomWidth: 1, borderBottomColor: '#FFD700', paddingBottom: 10 },
  image: { width: 300, height: 200, borderRadius: 10 },
  name: { fontSize: 22, color: '#FFD700', fontWeight: 'bold', marginTop: 5, fontFamily: 'playfair-bold' },
  price: { fontSize: 18, color: '#fff', fontFamily: 'playfair-display' },
});

6️⃣ screens/About.tsx
import React from 'react';
import { View, Text, StyleSheet, ScrollView } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { RootStackParamList } from '../App';

type Props = NativeStackScreenProps<RootStackParamList, 'About'>;

export default function About({}: Props) {
  return (
    <ScrollView style={styles.container}>
      <Text style={styles.title}>About Christophel’s Restaurant</Text>
      <Text style={styles.text}>
        Christophel’s Restaurant has been serving fine dining dishes for over 20 years. Our chefs use the freshest ingredients
        to create an unforgettable culinary experience. Join us for an evening of elegance, exquisite flavors, and excellent service.
      </Text>
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#000', padding: 20 },
  title: { fontSize: 28, fontWeight: 'bold', color: '#FFD700', marginBottom: 10, fontFamily: 'playfair-bold' },
  text: { fontSize: 16, color: '#fff', lineHeight: 24, fontFamily: 'playfair-display' },
});

7️⃣ screens/Enquiry.tsx
import React, { useState } from 'react';
import { View, Text, TextInput, StyleSheet, Alert, ScrollView } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { RootStackParamList } from '../App';
import GoldButton from '../components/GoldButton';

type Props = NativeStackScreenProps<RootStackParamList, 'Enquiry'>;

export default function Enquiry({}: Props) {
  const [name, setName] = useState<string>('');
  const [email, setEmail] = useState<string>('');
  const [message, setMessage] = useState<string>('');

  const handleSubmit = () => {
    if (!name || !email || !message) {
      Alert.alert('Error', 'All fields are required!');
      return;
    }
    const emailRegex = /\S+@\S+\.\S+/;
    if (!emailRegex.test(email)) {
      Alert.alert('Error', 'Please enter a valid email!');
      return;
    }
    Alert.alert('Success', 'Enquiry submitted successfully!');
    setName(''); setEmail(''); setMessage('');
  };

  return (
    <ScrollView style={styles.container}>
      <Text style={styles.title}>Enquiry Form</Text>
      <TextInput style={styles.input} placeholder="Name" placeholderTextColor="#ccc" value={name} onChangeText={setName} />
      <TextInput style={styles.input} placeholder="Email" placeholderTextColor="#ccc" value={email} onChangeText={setEmail} keyboardType="email-address" />
      <TextInput style={[styles.input, { height: 100 }]} placeholder="Message" placeholderTextColor="#ccc" value={message} onChangeText={setMessage} multiline />
      <GoldButton title="Submit" onPress={handleSubmit} />
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#000', padding: 20 },
  title: { fontSize: 28, fontWeight: 'bold', color: '#FFD700', marginBottom: 20, fontFamily: 'playfair-bold' },
  input: {
    borderWidth: 1,
    borderColor: '#FFD700',
    borderRadius: 12,
    padding: 12,
    marginBottom: 15,
    color: '#fff',
    fontFamily: 'playfair-display',
    fontSize: 16,
  },
});

8️⃣ screens/Gallery.tsx
import React from 'react';
import { ScrollView, Image, StyleSheet } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { RootStackParamList } from '../App';

type Props = NativeStackScreenProps<RootStackParamList, 'Gallery'>;

const images: string[] = [
  'https://images.unsplash.com/photo-1541544180-63a6c7dc9b26?fit=crop&w=800&q=80',
  'https://images.unsplash.com/photo-1600891964599-03a23f3f4e5c?fit=crop&w=800&q=80',
  'https://images.unsplash.com/photo-1600891964599-72d5b6a8b6d2?fit=crop&w=800&q=80',
  'https://images.unsplash.com/photo-1565299624946-b28f40a0ae38?fit=crop&w=800&q=80',
];

export default function Gallery({}: Props) {
  return (
    <ScrollView style={styles.container}>
      {images.map((uri, index) => (
        <Image key={index} source={{ uri }} style={styles.image} />
      ))}
    </ScrollView>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#000', padding: 10 },
  image: { width: '100%', height: 200, marginBottom: 15, borderRadius: 10 },
});

9️⃣ screens/Reservation.tsx
import React from 'react';
import { View, Text, StyleSheet } from 'react-native';
import { NativeStackScreenProps } from '@react-navigation/native-stack';
import { RootStackParamList } from '../App';

type Props = NativeStackScreenProps<RootStackParamList, 'Reservation'>;

export default function Reservation({}: Props) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Reservation Page</Text>
      <Text style={styles.text}>
        Coming soon! You will be able to book tables directly from the app.
      </Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#000', justifyContent: 'center', alignItems: 'center', padding: 20 },
  title: { fontSize: 28, fontWeight: 'bold', color: '#FFD700', marginBottom: 10, fontFamily: 'playfair-bold' },
  text: { fontSize: 18, color: '#fff', textAlign: 'center', fontFamily: 'playfair-display' },
});
  title: { fontSize: 36, fontWeight: 'bold', color: '#FFD700', marginBottom: 10, fontFamily: 'playfair-bold' },
  subtitle: { fontSize: 20, color: '#fff', marginBottom: 20, fontFamily: 'playfair-display' },
});
