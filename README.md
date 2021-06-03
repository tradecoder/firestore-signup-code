# firestore-signup-code
Signup code with firestore and react native

```javascript
import React, {useState} from 'react';
import {ThemeProvider, Text, Input, Button} from 'react-native-elements';
import Icon from 'react-native-vector-icons/FontAwesome';
import {TouchableOpacity, Linking} from 'react-native';
import {KeyboardAwareScrollView} from 'react-native-keyboard-aware-scrollview';
import { SafeAreaProvider } from 'react-native-safe-area-context';
import {firebase} from '../firebase/config';


export default function Signup({nav}){
  const [firstName, setFirstName]= useState("");
  const [lastName, setLastName] =  useState("");
  const [email, setEmail] = useState("");
  const [mobile, setMobile] = useState("");
  const [password, setPassword] = useState("");
  function onChangeFirstName(e){
    setFirstName(e)
  }
  function onChangeLastName(e){
    setLastName(e)
  }
  function onChangeEmail(e){
    setEmail(e)
  }
  function onChangeMobile(e){
    setMobile(e)
  }
  function onChangePassword(e){
    setPassword(e)
  }

  function onPressSignup(e){
    firebase.auth()
    .createUserWithEmailAndPassword(email, password)
    .then(response=>{
      const uid = response.user.uid;
      const data = {_id:uid, firstName, lastName, email, mobile};

      const usersRef= firebase.firestore().collection('users');
      usersRef.doc(uid)
      .set(data)
      .then(()=>alert("success")) // change this line based on your project requirement 
      .catch(err=>alert(err))
    })
    .catch(err=>alert(err))
  }
  
  return(
  <SafeAreaProvider>
    <ThemeProvider>
    <KeyboardAwareScrollView>
      <Text h2>Please Signup</Text>
      <Input placeholder='First name' onChangeText={onChangeFirstName} />
      <Input placeholder='Last name' onChangeText={onChangeLastName} />
      <Input placeholder='Email address' onChangeText={onChangeEmail} leftIcon={{ type: 'font-awesome', name:'envelope' }}/>
      <Input placeholder='Mobile number' keyboardType="number-pad" maxLength={11} onChangeText={onChangeMobile} leftIcon={{ type: 'font-awesome', name:'phone' }}/>
      <Input placeholder='Password' onChangeText={onChangePassword} secureTextEntry={true} leftIcon={{ type: 'font-awesome', name:'lock'}}/>
     </KeyboardAwareScrollView>
      <Button title="Signup" onPress={onPressSignup}/>
      <Text>
        <Text> Have an account?</Text>
        <TouchableOpacity onPress={()=>Linking.openURL("https://google.com/")}>
          <Text style={{color:"blue"}}> Login here</Text>
        </TouchableOpacity>                
      </Text>      
    </ThemeProvider>
   </SafeAreaProvider>
  )
}
```



