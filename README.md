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
    print('\nInventory System');
    print('1. Add Product');
    print('2. View Products');
    print('3. Sell Product');
    print('4. Exit');
    stdout.write('Enter choice: ');
    String? choice = stdin.readLineSync();
switch (choice) {
      case '1':
        // Add Product
        stdout.write('Enter product name: ');
        String name = stdin.readLineSync() ?? '';

        stdout.write('Enter price: ');
        double price = double.tryParse(stdin.readLineSync() ?? '0') ?? 0;

        stdout.write('Enter quantity: ');
        int quantity = int.tryParse(stdin.readLineSync() ?? '0') ?? 0;

        inventory.add(Product(name, price, quantity));
        print('Product added successfully.');
        break;

      case '2':
        // View Products
        if (inventory.isEmpty) {
          print('No products in inventory.');
        } else {
          print('\n| Product Name      | Price    | Quantity |');
          print('|-------------------|----------|----------|');
          for (var product in inventory) {
            print(product);
          }
        }
        break;

      case '3':
        // Sell Product
        if (inventory.isEmpty) {
          print('No products available to sell.');
          break;
        }

        stdout.write('Enter product name to sell: ');
        String name = stdin.readLineSync() ?? '';

        Product? product = inventory.firstWhere(
          (p) => p.name.toLowerCase() == name.toLowerCase(),
          orElse: () => Product('', 0, 0),
        );

        if (product.name.isEmpty) {
          print('Product not found.');
          break;
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
        break;

      case '4':
        // Exit
        isRunning = false;
        print('Exiting...');
        break;

      default:
        print('Invalid choice. Please try again.');
    }
  }
}
    
