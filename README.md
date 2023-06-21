import javafx.application.Application;
import javafx.collections.FXCollections;
import javafx.collections.ObservableList;
import javafx.geometry.Insets;
import javafx.geometry.Pos;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.GridPane;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;
import javafx.scene.image.Image;
import javafx.scene.image.ImageView;
import javafx.scene.control.ListView;
import javafx.scene.control.Spinner;
import javafx.scene.control.SpinnerValueFactory;
import javafx.scene.control.TextArea;
import javafx.scene.control.Alert;


/**
 *
 * @author Hazmi , Mohamad Akif Hakimi , Aisyah Afiqah , Faheyra Ezzah 
 */
public class McDonaldsOrderingSystem extends Application {
    
                 
               // GUI components
    
    private ComboBox<String> categoryComboBox;  // ComboBox to select the category
    private ComboBox<MenuItem> menuComboBox;  // ComboBox to select the menu item
    private Spinner<Integer> quantitySpinner;  // Spinner to select the quantity
    private ListView<OrderItem> orderListView;  // ListView to display the order items
    private Label totalBillLabel; // Label to display the total bill
    private Order order;  // Instance of the Order class
    private Button confirmOrderButton;  // Button to finalize the order

           // Menu item arrays for different categories
    
    private MenuItem[] burgerItems = {
            new MenuItem("Cheeseburger", 6.50),
            new MenuItem("Big Mac", 7.00),
            new MenuItem("Spicy Chicken McDeluxe", 14.50),
            new MenuItem("Spicy Beef Deluxe", 11.90),
            new MenuItem("Double Cheeseburger", 11.50)
    };

    private MenuItem[] chickenItems = {
            new MenuItem("Spicy Chicken", 7.99),
            new MenuItem("Normal Chicken", 6.99)
    };

    private MenuItem[] dessertItems = {
            new MenuItem("Oreo McFlurry", 6.00),
            new MenuItem("Apple Pie", 4.50),
            new MenuItem("Sundae Cone", 2.90)
    };

    private MenuItem[] sideItems = {
            new MenuItem("French Fries", 2.99),
            new MenuItem("Nuggets 8 pcs", 8.00)
    };

    public static void main(String[] args) {
        launch(args);
    }

    @Override
   public void start(Stage primaryStage) {

    order = new Order();  // Create a new instance of the Order class

    GridPane gridPane = new GridPane();     // Create the main grid pane
    gridPane.setAlignment(Pos.CENTER);
    gridPane.setPadding(new javafx.geometry.Insets(10));
    gridPane.setHgap(10);
    gridPane.setVgap(10);

  
             // Create welcome label and start ordering button
    Label welcomeLabel = new Label("Welcome to McDonald's!");
    welcomeLabel.setStyle("-fx-font-size: 18px; -fx-font-weight: bold;");

    Button startOrderingButton = new Button("Start Ordering");
    startOrderingButton.setOnAction(e -> showOrderingPage(primaryStage));

          // Create a VBox to hold the welcome label and start ordering button
    VBox vbox = new VBox(10);
    vbox.setAlignment(Pos.CENTER);
    vbox.setPadding(new javafx.geometry.Insets(10));
    vbox.getChildren().addAll( welcomeLabel, startOrderingButton);

    gridPane.add(vbox, 0, 0);

           // Create the scene and set it to the primary stage
    Scene scene = new Scene(gridPane, 400, 500);
    primaryStage.setScene(scene);
    primaryStage.setTitle("McDonald's Ordering System");
    primaryStage.show();
    
    ImageView logoImageView = new ImageView();
    Image logoImage = new Image("mcdonals_logo.jpg");
    logoImageView.setImage(logoImage);
    logoImageView.setFitWidth(300);
    logoImageView.setFitHeight(300);
    vbox.getChildren().add(0, logoImageView);
}

    private void showOrderingPage(Stage primaryStage) {
        
        order = new Order();    // Create a new instance of the Order class

            // Create the grid pane for the ordering page
            
        GridPane gridPane = new GridPane();   
        gridPane.setPadding(new javafx.geometry.Insets(10));
        gridPane.setHgap(10);
        gridPane.setVgap(10);
        
            // Create the category label and combo box 
            
        Label categoryLabel = new Label("Category:");  
        categoryComboBox = new ComboBox<>();
        categoryComboBox.getItems().addAll("Burger", "Chicken", "Dessert", "Side");
        categoryComboBox.setPromptText("Select Category");
        categoryComboBox.setOnAction(e -> populateMenuComboBox());

             // Create the menu label and combo box
                
        Label menuLabel = new Label("Menu:");
        menuComboBox = new ComboBox<>();
        menuComboBox.setPromptText("Select Item");

             // Create the quantity label and spinner
             
        Label quantityLabel = new Label("Quantity:");
        SpinnerValueFactory<Integer> valueFactory = new SpinnerValueFactory.IntegerSpinnerValueFactory(1, 10, 1);
        quantitySpinner = new Spinner<>();
        quantitySpinner.setValueFactory(valueFactory);
        
          // Create the add button and its event handler 

        Button addButton = new Button("Add to Order");
        addButton.setOnAction(e -> addItemToOrder());

              // Create the order list view
              
        orderListView = new ListView<>();
        orderListView.setPrefHeight(150);
        
                 
        // Create the remove button and its event handler

        Button removeButton = new Button("Remove from Order");
        removeButton.setOnAction(e -> removeItemFromOrder());

           // Create the Confirm order button and its event handler
         confirmOrderButton = new Button("Confirm Order");
         confirmOrderButton.setOnAction(e -> confirmOrder(primaryStage));
        
               // Create the print receipt button and its event handler

        Button printReceiptButton = new Button("Print Receipt");
       printReceiptButton.setOnAction(e -> printReceipt(primaryStage));

                   // Create the total bill label
        totalBillLabel = new Label("Total Bill: RM0.00");

        VBox vbox = new VBox(10);    // Create a VBox to hold all the components
        vbox.setPadding(new javafx.geometry.Insets(10));
        vbox.getChildren().addAll(categoryLabel, categoryComboBox, menuLabel, menuComboBox, quantityLabel, quantitySpinner,
                addButton, orderListView, removeButton, confirmOrderButton, printReceiptButton, totalBillLabel);

        gridPane.add(vbox, 0, 0);  // Add the VBox to the grid pane
        
         // Create the scene and set it to the primary stage
        Scene scene = new Scene(gridPane, 400, 500);
        primaryStage.setScene(scene);
        primaryStage.setTitle("McDonald's Ordering System");
        primaryStage.show();
    }

    private void populateMenuComboBox() {
        String selectedCategory = categoryComboBox.getValue();
        MenuItem[] items = null;

        // Based on the selected category, assign the corresponding array of menu items
        switch (selectedCategory) {
            case "Burger":
                items = burgerItems;
                break;
            case "Chicken":
                items = chickenItems;
                break;
            case "Dessert":
                items = dessertItems;
                break;
            case "Side":
                items = sideItems;
                break;
        }

        if (items != null) {
            menuComboBox.getItems().setAll(items);
    }
    }

  private void addItemToOrder() {
    MenuItem selectedItem = menuComboBox.getValue(); // Get the selected menu item from the combo box
    if (selectedItem == null) {
        showAlert("No Item Selected", "Please select an item to add.", Alert.AlertType.WARNING);
        return;
    }
    
    int quantity = quantitySpinner.getValue(); // Get the selected quantity from the spinner
    order.addItem(selectedItem, quantity); // Add the selected item with the specified quantity to the order
    orderListView.setItems(order.getItems()); // Update the order items in the list view
    totalBillLabel.setText("Total Bill: RM" + String.format("%.2f", order.calculateTotalBill())); // Update the total bill displayed
    showAlert("Item Added", "The item has been added to the order.", Alert.AlertType.INFORMATION);
}

private void removeItemFromOrder() {
    int selectedIndex = orderListView.getSelectionModel().getSelectedIndex(); // Get the index of the selected item in the list view
    if (selectedIndex >= 0) {
        order.removeItem(selectedIndex); // Remove the selected item from the order
        orderListView.setItems(order.getItems()); // Update the order items in the list view
        totalBillLabel.setText("Total Bill: RM" + String.format("%.2f", order.calculateTotalBill())); // Update the total bill displayed
        showAlert("Item Removed", "The item has been removed from the order.", Alert.AlertType.INFORMATION);
    } else {
        showAlert("No Item Selected", "Please select an item to remove.", Alert.AlertType.WARNING); 
    }
}

private void showAlert(String title, String message, Alert.AlertType alertType) {
    Alert alert = new Alert(alertType);
    alert.setTitle(title);
    alert.setHeaderText(null);
    alert.setContentText(message);
    alert.showAndWait();
}

   private void confirmOrder(Stage primaryStage) {
    if (order.getItems().isEmpty()) {
        showAlert("Empty Order", "No items in the order.", Alert.AlertType.ERROR);
    } else {
        showAlert("Order Complete ! ", "The order has been confirmed.", Alert.AlertType.INFORMATION);
        order.confirm();  // Set the confirmed flag of the Order object to true
        confirmOrderButton.setDisable(true);
}
}

   private void printReceipt(Stage primaryStage) {
    if (!order.isConfirmed()) {
        showAlert("Order Confirmation Required", "Please confirm your order before printing the receipt.", Alert.AlertType.ERROR);
    } else if (order.getItems().isEmpty()) {
        showAlert("Empty Order", "No items in the order.", Alert.AlertType.ERROR);
    } else {
        StringBuilder receiptText = new StringBuilder();
        for (OrderItem item : order.getItems()) {
            receiptText.append(item).append("\n");
        }

        receiptText.append("--------------------\n");
        receiptText.append("Total Bill: RM").append(order.calculateTotalBill()).append("\n");

        TextArea receiptTextArea = new TextArea(receiptText.toString());
        receiptTextArea.setEditable(false);
        receiptTextArea.setPrefRowCount(10);

        Button closeButton = new Button("Close");
        closeButton.setOnAction(e -> {
            primaryStage.close();
            order.clear();
            confirmOrderButton.setDisable(false);
        });

        VBox receiptLayout = new VBox(10);
        receiptLayout.setPadding(new Insets(10));
        receiptLayout.getChildren().addAll(receiptTextArea, closeButton);

        Scene receiptScene = new Scene(receiptLayout, 300, 400);
        Stage receiptStage = new Stage();
        receiptStage.setScene(receiptScene);
        receiptStage.setTitle("Receipt");
        receiptStage.show();
}
}

    private void updateTotalBill() {
        double totalBill = order.calculateTotalBill();
        totalBillLabel.setText("Total Bill: RM" + String.format("%.2f", totalBill));
    }}

class MenuItem {
    private String name;
    private double price;

    public MenuItem(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public String getName() {
        return name;
    }

    public double getPrice() {
        return price;
    }

    @Override
    public String toString() {
        return name + " (RM" + price + ")";
}
}

class OrderItem extends MenuItem {
    int quantity;

    public OrderItem(String name, double price, int quantity) {
        super(name, price);
        this.quantity = quantity;
    }

    public int getQuantity() {
        return quantity;
    }

    public double getItemTotal() {
        return getPrice() * quantity;
    }

    @Override
    public String toString() {
        return getName() + " - Quantity: " + quantity + " - Subtotal: RM" + getItemTotal();
}
}

class Order {
    private ObservableList<OrderItem> items;
    private boolean confirmed;

    public Order() {
        items = FXCollections.observableArrayList();
        confirmed = false;
    }

    public ObservableList<OrderItem> getItems() {
        return items;
    }

    public void addItem(MenuItem item, int quantity) {
        OrderItem orderItem = new OrderItem(item.getName(), item.getPrice(), quantity);
        items.add(orderItem);
    }

    public void removeItem(int index) {
        items.remove(index);
    }

    public double calculateTotalBill() {
        double totalBill = 0.0;
        for (OrderItem item : items) {
            totalBill += item.getItemTotal();
        }
        return totalBill;
    }

    public boolean isConfirmed() {
        return confirmed;
    }

    public void confirm() {
        confirmed = true;
    }

    public void clear() {
        items.clear();
        confirmed = false;
}
}
