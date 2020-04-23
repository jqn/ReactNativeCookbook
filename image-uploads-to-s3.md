# Image Uploads to S3

{% embed url="https://medium.com/@istvanistvan/react-native-custom-image-picker-with-upload-progress-5444e955455c" %}

#### [https://dev.to/provish/upload-image-directly-to-s3-bucket-from-client-ffg](https://dev.to/provish/upload-image-directly-to-s3-bucket-from-client-ffg)

#### AWS S3 

```markup
<?xml version=”1.0" encoding=”UTF-8"?>
<CORSConfiguration>
<CORSRule>
 <AllowedOrigin>*</AllowedOrigin>
 <AllowedMethod>POST</AllowedMethod>
 <AllowedMethod>GET</AllowedMethod>
 <AllowedMethod>PUT</AllowedMethod>
 <AllowedMethod>DELETE</AllowedMethod>
 <AllowedMethod>HEAD</AllowedMethod>
 <AllowedHeader>*</AllowedHeader>
</CORSRule>
</CORSConfiguration>
```

#### Expose an endpoint to get signed URL

```jsx
// server
// npm install aws-sdk

const express = require("express");
const app = express();
const port = 3000;

const AWS = require("aws-sdk");
const s3 = new AWS.S3({
  accessKeyId: "<aws_access_key_id>", // aws access id here
  secretAccessKey: "<aws_secret_access_key>", // aws secret access key here
  useAccelerateEndpoint: true
});
const params = {
  Bucket: "<Bucket Name>",
  Key: "<Put your key here>",
  Expires: 60*60, // expiry time
  ACL: "bucket-owner-full-control",
  ContentType: "image/jpeg" // this can be changed as per the file type
};

// api endpoint to get signed url
app.get("/get-signed-url", (req, res) => {
  const fileurls = [];
  s3.getSignedUrl("putObject", params, function(err, url) {
    if (err) {
      console.log("Error getting presigned url from AWS S3");
      res.json({
        success: false,
        message: "Pre-Signed URL error",
        urls: fileurls
      });
    } else {
      fileurls[0] = url;
      console.log("Presigned URL: ", fileurls[0]);
      res.json({
        success: true,
        message: "AWS SDK S3 Pre-signed urls generated successfully.",
        urls: fileurls
      });
    }
  });
});

app.listen(port, () => console.log(`Server listening on port ${port}!`));
```

#### Upload Screen

```jsx
import { CameraRoll, TouchableOpacity, View } from 'react-native';
....
state = { photos: null, selectedPhoto: null };
componentDidMount() {
  const { navigation } = this.props;
   navigation.setParams({ upload: this.upload });
   this.getPhotosAsync({ first: 100 });
}
async getPhotosAsync(params) {
  return new Promise((res, rej) =>
    CameraRoll.getPhotos(params)
      .then(data => {
        const assets = data.edges;
        const photos = assets.map(asset => asset.node.image);
           this.setState({ photos });
           res({ photos });
      })
      .catch(rej)
  );
}
```

#### Image Picker Component

```jsx
<FlatList
  horizontal
  showsHorizontalScrollIndicator={false}
  renderItem={photo => {
    return this.renderPhoto(photo);
  }}
  keyExtractor={(photo, index) => index.toString()}
  data={photos}
/>
```

#### renderPhoto Callback

```jsx
renderPhoto = photo => {
  return (
    <TouchableOpacity
      onPress={() => {
        this.pickPhoto(photo.item);
      }}
    >
      <Image source={{ uri: photo.item.uri }} style={styles.gallerImage} />
    </TouchableOpacity>
  );
};
```

#### ImagePicker.js

```jsx
import React from 'react';
import PropTypes from 'prop-types';
import { View, Image, FlatList, Text, TouchableOpacity, StyleSheet, Animated } from 'react-native';

const styles = StyleSheet.create({
    flexContainer: {
        flex: 1,
        backgroundColor: '#000',
    },
    flatListContainer: {
        justifyContent: 'flex-end',
        height: 120,
    },
    galleryImage: {
        width: 120,
        height: 120,
    },
});

export default class CapaImagePicker extends React.Component {
    state = { selectedPhoto: null, bounceValue: new Animated.Value(100) };

    componentDidMount() {
        const { photos } = this.props;
        this.pickPhoto(photos[0]);
    }

    pickPhoto(photo) {
        const { onChange } = this.props;
        this.setState({ selectedPhoto: photo });
        onChange(photo);
    }

    renderPhoto = photo => {
        return (
            <TouchableOpacity
                onPress={() => {
                    this.pickPhoto(photo.item);
                }}
            >
                <Image source={{ uri: photo.item.uri }} style={ styles.galleryImage } />
            </TouchableOpacity>
        );
    };

    toggleGallery() {
        const { bounceValue } = this.state;
        Animated.spring(bounceValue, {
            toValue: 220,
            velocity: 3,
            tension: 2,
            friction: 8,
            useNativeDriver: true,
        }).start();
    }

    render() {
        const { selectedPhoto, bounceValue } = this.state;
        const { photos } = this.props;
        return (
            <View style={styles.flexContainer}>
                <View style={styles.flexContainer}>
                    {selectedPhoto && (
                        <Image
                            resizeMode="contain"
                            source={{ uri: selectedPhoto.uri }}
                            style={{ flex: 1 }}
                        />
                    )}
                </View>
                <View style={styles.flatListContainer}>
                    {photos ? (
                        <Animated.View
                            style={[{ height: 220 }, { transform: [{ translateY: bounceValue }] }]}
                        >
                            <FlatList
                                horizontal
                                showsHorizontalScrollIndicator={false}
                                renderItem={photo => {
                                    return this.renderPhoto(photo);
                                }}
                                keyExtractor={(photo, index) => index.toString()}
                                data={photos}
                            />
                        </Animated.View>
                    ) : (
                        <Text>Fetching photos...</Text>
                    )}
                </View>
            </View>
        );
    }
}

CapaImagePicker.propTypes = {
    onChange: PropTypes.func.isRequired,
    photos: PropTypes.arrayOf(PropTypes.shape({})).isRequired,
};
```

#### Upload progress component

```jsx
import React from 'react';
import { View, StyleSheet, Text } from 'react-native';
import { Icon } from 'react-native-elements';
import * as Progress from 'react-native-progress';
import { vw } from 'react-native-expo-viewport-units';
import AnimatedEllipsis from 'react-native-animated-ellipsis';
import PropTypes from 'prop-types';

const styles = StyleSheet.create({
    container: {
        flex: 1,
        flexDirection: 'row',
        marginBottom: 10,
    },
    itemAlignStart: {
        flexDirection: 'row',
    },
    itemAlignEnd: {
        marginLeft: 'auto',
    },
    progressText: {
        paddingTop: 3,
        color: '#fff',
        paddingLeft: 10,
    },
    progressContainer: {
        color: '#fff',
        backgroundColor: '#000',
        paddingLeft: vw(5),
        paddingRight: vw(5),
        position: 'absolute',
        zIndex: 2,
    },
});

const CapaUploadProgress = props => {
    const { uploadFilename, uploadFileSize, uploadProgress } = props;

    return (
        <View style={styles.progressContainer}>
            <View style={styles.container}>
                <View style={styles.itemAlignStart}>
                    <Icon type="material" name="backup" color="#fff" />
                    <Text style={styles.progressText}>
                        {uploadFilename} ({Math.floor(uploadFileSize)} KB)
                    </Text>
                </View>
                <View style={styles.itemAlignEnd}>
                    <AnimatedEllipsis
                        style={{
                            color: '#fff',
                            fontSize: 30,
                            letterSpacing: 0,
                            marginTop: -15,
                        }}
                    />
                </View>
            </View>
            <Progress.Bar
                unfilledColor="#000"
                borderRadius={0}
                borderWidth={0}
                height={1}
                color="white"
                progress={uploadProgress}
                width={vw(90)}
            />
        </View>
    );
};

CapaUploadProgress.propTypes = {
    uploadFilename: PropTypes.string.isRequired,
    uploadFileSize: PropTypes.number.isRequired,
    uploadProgress: PropTypes.number.isRequired,
};

export default CapaUploadProgress;
```

#### Call a method in ImagePicker component

```jsx
constructor() {
   super();
   this.imagePicker = React.createRef();
}
```

#### Upload Screen

```jsx
import React from 'react';
import { CameraRoll, TouchableOpacity, View } from 'react-native';
import { Icon } from 'react-native-elements';
import { connect } from 'react-redux';
import PropTypes from 'prop-types';
import * as S3Reducer from '../modules/s3/s3.reducer';
import CapaUploadProgress from '../components/CapaUploadProgress';
import CapaImagePicker from '../components/CapaImagePicker';
import { storePhoto } from '../modules/s3/s3.service';

class UploadScreen extends React.Component {
    static navigationOptions = ({ navigation }) => ({
        headerStyle: {
            backgroundColor: '#000',
            marginLeft: 15,
            marginRight: 15,
            borderBottomWidth: 0,
        },
        headerLeft: (
            <Icon
                name="close"
                color="#fff"
                onPress={() => {
                    navigation.goBack();
                }}
                Component={TouchableOpacity}
            />
        ),
        headerRight: !navigation.getParam('uploadProgress', 0) && (
            <Icon
                name="done"
                color="#fff"
                onPress={navigation.getParam('upload')}
                Component={TouchableOpacity}
            />
        ),
    });

    state = { photos: null, selectedPhoto: null };

    constructor() {
        super();
        this.imagePicker = React.createRef();
    }

    componentDidMount() {
        const { navigation } = this.props;
        navigation.setParams({ upload: this.upload });
        this.getPhotosAsync({ first: 100 });
    }

    async getPhotosAsync(params) {
        return new Promise((res, rej) =>
            CameraRoll.getPhotos(params)
                .then(data => {
                    const assets = data.edges;
                    const photos = assets.map(asset => asset.node.image);
                    this.setState({ photos });
                    res({ photos });
                })
                .catch(rej)
        );
    }

    upload = () => {
        const { navigation } = this.props;
        const { dispatchStorePhoto } = this.props;
        const { selectedPhoto } = this.state;
        navigation.setParams({ uploadProgress: 1 });
        this.imagePicker.current.toggleGallery();
        dispatchStorePhoto(selectedPhoto);
    };

    imagePickerChange(photo) {
        this.setState({ selectedPhoto: photo });
    }

    render() {
        const { photos } = this.state;
        const { uploadProgress, uploadFilename, uploadFileSize } = this.props;
        const progressProps = { uploadProgress, uploadFilename, uploadFileSize };
        return (
            <View style={{ flex: 1 }}>
                {uploadProgress && <CapaUploadProgress {...progressProps} />}
                {photos && (
                    <CapaImagePicker
                        ref={this.imagePicker}
                        onChange={photo => this.imagePickerChange(photo)}
                        photos={photos}
                    />
                )}
            </View>
        );
    }
}

UploadScreen.propTypes = {
    uploadProgress: PropTypes.number,
    uploadFileSize: PropTypes.number,
    uploadFilename: PropTypes.string,
    dispatchStorePhoto: PropTypes.func.isRequired,
    navigation: PropTypes.shape({ navigate: PropTypes.func.isRequired }).isRequired,
};

UploadScreen.defaultProps = {
    uploadProgress: null,
    uploadFileSize: null,
    uploadFilename: null,
};

function mapStateToProps(store) {
    return {
        uploadProgress: store.s3.uploadProgress,
        uploadFilename: store.s3.uploadFilename,
        uploadFileSize: store.s3.uploadFileSize,
    };
}

function mapDispatchToProps(dispatch) {
    return {
        dispatchStorePhoto: photo => {
            dispatch(
                storePhoto(photo, {
                    userId: 1,
                    caption: 'New Mexico',
                    lat: 36.2921,
                    long: 108.1298,
                })
            ).then(() => {
                dispatch(S3Reducer.setUploadProgress(null));
            });
        },
    };
}

const UploadConnect = connect(
    mapStateToProps,
    mapDispatchToProps
)(UploadScreen);

export default UploadConnect;
```

{% embed url="https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingAWSSDK.html" %}

{% embed url="https://www.digitalocean.com/community/questions/upload-aws-s3-getsignedurl-with-correct-permissions-and-content-type" %}

{% embed url="https://techsparx.com/software-development/aws/aws-sdk-promises.html" %}

{% embed url="https://docs.ncloud.com/ko/storage/storage-8-4.html" %}

{% embed url="https://gist.github.com/rxgx/7e1b24de5936ff1b2b815a3d9cc3897a" %}

```text
import {useState, useEffect} from 'react';
import axios from 'axios';

const ACCESS_KEY = 'QLxPz_PBR33NLxX2EkJQ8L_nelZcYEXGZCrFtNQMSTs';

const useAsyncHook = query => {
  const [result, setResult] = useState([]);
  const [loading, setLoading] = useState('false');

  useEffect(() => {
    async function fetchCarsList() {
      try {
        setLoading('true');
        const response = await axios.get(
          `https://api.unsplash.com/search/photos/?client_id=${ACCESS_KEY}&query=${query}`,
        );
        const json = await response.data.results;
        console.log('fetching ****** ', json);
        setResult(json);
      } catch (error) {
        console.log('fetch error', error);
        setLoading('null');
      }
    }

    if (query !== '') {
      fetchCarsList();
    }
  }, [query]);

  return [result, loading];
};

export default useAsyncHook;
```

