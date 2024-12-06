import React, { useState } from 'react';
import {
  StyleSheet,
  Text,
  View,
  TouchableOpacity,
  TextInput,
  FlatList,
  Alert,
} from 'react-native';

export default function App() {
  const [products, setProducts] = useState([]);
  const [showForm, setShowForm] = useState(false);
  const [productName, setProductName] = useState('');
  const [productPrice, setProductPrice] = useState('');
  const [totalPrice, setTotalPrice] = useState(0);

  const handleAddProduct = () => {
    setShowForm(true);
  };

  const handleSubmit = () => {
    // Validasi input
    if (!productName || !productPrice) {
      Alert.alert('Error', 'Nama produk dan harga harus diisi!');
      return;
    }
    if (isNaN(productPrice)) {
      Alert.alert('Error', 'Harga harus berupa angka!');
      return;
    }

    // Tambahkan produk ke daftar
    setProducts([...products, { name: productName, price: parseFloat(productPrice) }]);
    setTotalPrice(totalPrice + parseFloat(productPrice));
    setProductName('');
    setProductPrice('');
    setShowForm(false);
  };

  const handleRemoveProduct = (index) => {
    const selectedProduct = products[index];
    setTotalPrice(totalPrice - selectedProduct.price);

    // Hapus produk berdasarkan index
    setProducts(products.filter((_, i) => i !== index));
  };

  return (
    <View style={styles.container}>
      {/* Header */}
      <View style={styles.header}>
        <Text style={styles.headerText}>Produk</Text>
        <TouchableOpacity style={styles.addButton} onPress={handleAddProduct}>
          <Text style={styles.buttonText}>TAMBAH</Text>
        </TouchableOpacity>
        <Text style={styles.totalPrice}>{totalPrice}</Text>
      </View>

      {/* Form Input */}
      {showForm && (
        <View style={styles.form}>
          <TextInput
            style={styles.input}
            placeholder="Nama Produk"
            value={productName}
            onChangeText={setProductName}
          />
          <TextInput
            style={styles.input}
            placeholder="Harga"
            keyboardType="numeric"
            value={productPrice}
            onChangeText={setProductPrice}
          />
          <TouchableOpacity style={styles.okButton} onPress={handleSubmit}>
            <Text style={styles.buttonText}>OKE</Text>
          </TouchableOpacity>
        </View>
      )}

      {/* List Produk */}
      <FlatList
        data={products}
        renderItem={({ item, index }) => (
          <TouchableOpacity
            style={styles.productItem}
            onPress={() => handleRemoveProduct(index)}
          >
            <Text style={styles.productText}>{item.name}</Text>
            <Text style={styles.productPrice}>{item.price}</Text>
          </TouchableOpacity>
        )}
        keyExtractor={(item, index) => index.toString()}
      />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#f5f5f5',
    padding: 10,
  },
  header: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    alignItems: 'center',
    marginBottom: 10,
  },
  headerText: {
    fontSize: 20,
    fontWeight: 'bold',
  },
  addButton: {
    backgroundColor: '#4CAF50', // Warna hijau
    padding: 10,
    borderRadius: 5,
  },
  buttonText: {
    color: '#fff',
    fontWeight: 'bold',
  },
  totalPrice: {
    fontSize: 18,
    fontWeight: 'bold',
  },
  form: {
    backgroundColor: '#fff',
    padding: 10,
    borderRadius: 5,
    marginBottom: 10,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    borderRadius: 5,
    padding: 10,
    marginBottom: 10,
  },
  okButton: {
    backgroundColor: '#4CAF50', 
    padding: 10,
    borderRadius: 5,
    alignItems: 'center',
  },
  productItem: {
    flexDirection: 'row',
    justifyContent: 'space-between',
    padding: 10,
    backgroundColor: '#fff',
    borderRadius: 5,
    marginBottom: 5,
  },
  productText: {
    fontSize: 16,
  },
  productPrice: {
    fontSize: 16,
    fontWeight: 'bold',
  },
});
