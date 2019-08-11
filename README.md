# react-native-inapp-updates
Google Play Store In-App Updates API (Flex Mode) Wrappper

# system required

Android 5.0+, API Version 21+

# demo

```
import React, { Component } from 'react'
import {
  View,
  Text,
  AppState,
  Alert,
  StyleSheet,
} from 'react-native'
import InappUpdates from 'react-native-inapp-updates'

export default class Demo extends Component {
  state = {
      updateAvailable: false,
      updateCheckFailed: false,
  }
  
  componentDidMount() {
    if (AppState.currentState == 'active') {
      this.checkUpdate()
    }
    
    AppState.addEventListener('change', this.handleAppStateChange)
  }
  
  componentWillUnmount() {
    AppState.removeEventListener('change', this.handleAppStateChange)
  }
  
  handleAppStateChange = (nextAppState) {
    if (nextAppState == 'active') {
      this.checkCompleted()
    }
  }
  
  checkUpdate = () {
    // InappUpdates.checkUpdate(requestCode?: int)
    InappUpdates.checkUpdate(1234567)
    .then(available => {
      his.setState({updateCheckFailed: false, updateAvailable: available})
    })
    .catch(error => {
      this.setState({updateCheckFailed: true, updateAvailable: false})
      console.warn('inapp-updates', error)
    })
  }
  
  checkCompleted = () => {
    const { updateCheckFailed, updateAvailable } = this.state
    if (updateCheckFailed || !updateAvailable) {
      this.checkUpdate()
    }
    else {
      InappUpdates.isDownloadCompleted().then(completed => {
        if (completed) {
          Alert.alert(
            "Update",
            "An update available",
            [
              {
                text: "Update",
                onPress: () => InappUpdates.completeUpdate(),
              },
              {
                text: "Later",
                onPress: () => {},
              },
            ],
            {cancelable: false}
          )
        }
      })
    }
  }
  
  render() {
    return <View style={styles.container}>
      <Text style={styles.text}>react-native-inapp-updates demo.</Text>
    </View>
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: 'center',
    justifyContent: 'center',
    backgroundColor: '#F7F8F9',
  },
  text: {
    fontSize: 18,
  },
})
```
