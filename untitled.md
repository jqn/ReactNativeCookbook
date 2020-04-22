---
description: Build a photo gallery
---

# FlatList

Photo gallery

```text
// https://dev.to/vinodchauhan7/react-hooks-with-async-await-1n9g
// https://medium.com/react-native-development/how-to-use-the-flatlist-component-react-native-basics-92c482816fe6
// http://www.reactnativeexpress.com/flatlist
// https://hackernoon.com/how-to-highlight-and-multi-select-items-in-a-flatlist-component-react-native-1ca416dec4bc
// https://stackoverflow.com/questions/59486602/react-native-flatlist-delete-item
// https://snack.expo.io/@spencercarli/react-native-flatlist-grid
import React, {useState, useEffect} from 'react';
import {View, Text} from 'react-native';
import {connect} from 'react-redux';

import MultiColumnGallery from '../../Components/Gallery/MultiColumnGallery';
import useAsyncHook from '../../Hooks/useAsyncHook';

const ReportsUploadScreen = ({captures}) => {
  // const [search, setSearch] = useState('');
  // const [query, setQuery] = useState('');
  // const [result, loading] = useAsyncHook(query);

  // useEffect(() => {
  //   setQuery('cars');
  // }, []);

  // console.log('loading', loading);
  // if (loading === false) {
  //   return (
  //     <View style={{flex: 1}}>
  //       <Text>Ready to search</Text>
  //     </View>
  //   );
  // }
  // if (loading === 'null') {
  //   return (
  //     <View style={{flex: 1}}>
  //       <Text>No results</Text>
  //     </View>
  //   );
  // }
  return (
    <View style={{flex: 1}}>
      <MultiColumnGallery data={captures} />
    </View>
  );
};

const mapStateToProps = state => ({
  captures: state.reportCaptures,
});

export default connect(mapStateToProps)(ReportsUploadScreen);

```

```text
import React from 'react';
import {StyleSheet, Text, View, FlatList, Dimensions} from 'react-native';

const data = [
  {key: 'A'},
  {key: 'B'},
  {key: 'C'},
  {key: 'D'},
  {key: 'E'},
  {key: 'F'},
  {key: 'G'},
  {key: 'H'},
  {key: 'I'},
  {key: 'J'},
  // { key: 'K' },
  // { key: 'L' },
];

const formatData = (data, numColumns) => {
  const numberOfFullRows = Math.floor(data.length / numColumns);

  let numberOfElementsLastRow = data.length - numberOfFullRows * numColumns;
  while (
    numberOfElementsLastRow !== numColumns &&
    numberOfElementsLastRow !== 0
  ) {
    data.push({key: `blank-${numberOfElementsLastRow}`, empty: true});
    numberOfElementsLastRow++;
  }

  return data;
};

const numColumns = 3;
export default class App extends React.Component {
  renderItem = ({item, index}) => {
    if (item.empty === true) {
      return <View style={[styles.item, styles.itemInvisible]} />;
    }
    return (
      <View style={styles.item}>
        <Text style={styles.itemText}>{item.key}</Text>
      </View>
    );
  };

  render() {
    return (
      <FlatList
        data={formatData(data, numColumns)}
        style={styles.container}
        renderItem={this.renderItem}
        numColumns={numColumns}
      />
    );
  }
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    // marginVertical: 20,
  },
  item: {
    backgroundColor: '#4D243D',
    alignItems: 'center',
    justifyContent: 'center',
    flex: 1,
    margin: 1,
    height: Dimensions.get('window').width / numColumns, // approximate a square
  },
  itemInvisible: {
    backgroundColor: 'transparent',
  },
  itemText: {
    color: '#fff',
  },
});

```

```text
import React from 'react';
import {
  FlatList,
  Image,
  StyleSheet,
  Text,
  TouchableOpacity,
  View,
} from 'react-native';

import PropTypes from 'prop-types';
import useRatio from '../../Hooks/useRatio';

const MultiColumnGallery = ({data, onPress}) => {
  const ratio = useRatio();
  // console.log('data', ratio.width / 3);
  return (
    <View style={styles.container}>
      <FlatList
        data={data}
        horizontal={false}
        numColumns={3}
        // ItemSeparatorComponent={() => <View style={styles.line} />}
        renderItem={({item}) => {
          if (item.empty === true) {
            return <View style={styles.image} />;
          }
          return (
            <TouchableOpacity style={styles.imageWrapper}>
              <Image
                source={{uri: item.urls.regular}}
                style={[styles.image]}
                resizeMode="cover"
              />
              <Text style={styles.label}>{item.label}</Text>
            </TouchableOpacity>
          );
        }}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    margin: 1,
  },
  // line: {
  //   height: 0.5,
  //   width: '100%',
  //   backgroundColor: '#000',
  // },
  image: {
    aspectRatio: 1,
    // width: '97%',
    // margin: '1.5%',
    // flex: 1,
    // width: null,
    // height: null,
    // maxWidth: '30%',
  },
  imageWrapper: {
    // backgroundColor: 'gray',
    aspectRatio: 1,
    // width: '100%',
    // padding: '1%',
    flex: 1,
    margin: 1,
    maxWidth: '33%',
    // borderWidth: 0.5,
    // borderColor: '#000',
  },
  label: {
    position: 'absolute',
    bottom: 0,
    zIndex: 100,
  },
});

MultiColumnGallery.defaultProps = {
  data: [],
  onPress: () => {},
};

MultiColumnGallery.propTypes = {
  data: PropTypes.array,
  onPress: PropTypes.func,
};

export default MultiColumnGallery;

```

