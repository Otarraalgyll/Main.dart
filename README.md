import 'dart:io';

class Product {
  String name;
  double price;
  int quantity;

  Product(this.name, this.price, this.quantity);

  @override
  String toString() {
    return '| ${name.padRight(16)} | ${price.toStringAsFixed(2).padLeft(8)} | ${quantity.toString().padLeft(8)} |';
  }
}

void main() {
  List<Product> inventory = [];
  bool isRunning = true;

  while (isRunning) {
    print('\nSelling ITEAMS');
    print('1. Add Product');
    print('2. View Products');
    print('3. Sell Product');
    print('4. Exit');
    stdout.write('Enter choice: ');
    String? choice = stdin.readLineSync();

    // ADD PRODUCT
    if (choice == '1') {
      stdout.write('Enter product name: ');
      String name = stdin.readLineSync() ?? '';

      stdout.write('Enter price: ');
      double price = double.tryParse(stdin.readLineSync() ?? '0') ?? 0;

      stdout.write('Enter quantity: ');
      int quantity = int.tryParse(stdin.readLineSync() ?? '0') ?? 0;

      inventory.add(Product(name, price, quantity));
      print('Product added successfully.');

    // VIEW PRODUCTS
    } else if (choice == '2') {
      if (inventory.isEmpty) {
        print('Inventory is empty.');
      } else {
        print('\n| Name             |   Price | Quantity |');
        print('-------------------------------------------');
        for (var product in inventory) {
          print(product);
        }
      }

    // SELL PRODUCT
    } else if (choice == '3') {
      stdout.write('Enter product name to sell: ');
      String name = stdin.readLineSync() ?? '';

      Product? product;
      try {
        product = inventory.firstWhere(
          (p) => p.name.toLowerCase() == name.toLowerCase(),
        );
      } catch (e) {
        product = null;
      }

      if (product == null) {
        print('Product not found.');
        continue;
      }

      stdout.write('Enter quantity to sell: ');
      int qtyToSell = int.tryParse(stdin.readLineSync() ?? '0') ?? 0;

      if (qtyToSell <= 0) {
        print('Invalid quantity.');
      } else if (product.quantity >= qtyToSell) {
        product.quantity -= qtyToSell;
        double amount = product.price * qtyToSell;
        print('Sold $qtyToSell ${product.name}(s). Amount: ${amount.toStringAsFixed(2)}');
        print('Remaining stock: ${product.quantity}');
      } else {
        print('Insufficient stock. Only ${product.quantity} available.');
      }

    // EXIT 
    } else if (choice == '4') {
      print('Exiting program...');
      isRunning = false;

    } else {
      print('Invalid choice. Try again.');
    }
  }
}

    
