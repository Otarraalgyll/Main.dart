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

    
