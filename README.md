public class Food {
    private int itemNo;
    private int quantity;
    private double price;

    public Food(int itemNo, int quantity, double price) {
        this.itemNo = itemNo;
        this.quantity = quantity;
        this.price = price;
    }

    public String getItemName() {
        return "Item " + itemNo;
    }

    public int getQuantity() {
        return quantity;
    }

    public double getPrice() {
        return price;
    }
}

public abstract class Room {
    protected String name;
    protected String contact;
    protected String gender;
    protected List<Food> foodItems;

    public Room(String name, String contact, String gender) {
        this.name = name;
        this.contact = contact;
        this.gender = gender;
        this.foodItems = new ArrayList<>();
    }

    public String getName() {
        return name;
    }

    public String getContact() {
        return contact;
    }

    public String getGender() {
        return gender;
    }

    public List<Food> getFoodItems() {
        return foodItems;
    }

    public void addFood(Food food) {
        foodItems.add(food);
    }

    public abstract void displayDetails();
}

public class SingleRoom extends Room {

    public SingleRoom(String name, String contact, String gender) {
        super(name, contact, gender);
    }

    @Override
    public void displayDetails() {
        System.out.println("Single Room Booking Details:");
        System.out.println("Name: " + name);
        System.out.println("Contact: " + contact);
        System.out.println("Gender: " + gender);
        System.out.println("Food Ordered: ");
        for (Food food : foodItems) {
            System.out.println(" - " + food.getItemName() + ", Quantity: " + food.getQuantity() + ", Price: " + food.getPrice());
        }
    }
}

public class DoubleRoom extends Room {
    private String name2;
    private String contact2;
    private String gender2;

    public DoubleRoom(String name, String contact, String gender, String name2, String contact2, String gender2) {
        super(name, contact, gender);
        this.name2 = name2;
        this.contact2 = contact2;
        this.gender2 = gender2;
    }

    public String getName2() {
        return name2;
    }

    public String getContact2() {
        return contact2;
    }

    public String getGender2() {
        return gender2;
    }

    @Override
    public void displayDetails() {
        System.out.println("Double Room Booking Details:");
        System.out.println("Primary Occupant - Name: " + name + ", Contact: " + contact + ", Gender: " + gender);
        System.out.println("Secondary Occupant - Name: " + name2 + ", Contact: " + contact2 + ", Gender: " + gender2);
        System.out.println("Food Ordered: ");
        for (Food food : foodItems) {
            System.out.println(" - " + food.getItemName() + ", Quantity: " + food.getQuantity() + ", Price: " + food.getPrice());
        }
    }
}

public class NotAvailable extends Exception {
    public NotAvailable(String message) {
        super(message);
    }
}

import java.util.ArrayList;

public class Holder {
    private ArrayList<SingleRoom> luxurySingleRooms;
    private ArrayList<SingleRoom> deluxeSingleRooms;
    private ArrayList<DoubleRoom> luxuryDoubleRooms;
    private ArrayList<DoubleRoom> deluxeDoubleRooms;

    public Holder() {
        luxurySingleRooms = new ArrayList<>();
        deluxeSingleRooms = new ArrayList<>();
        luxuryDoubleRooms = new ArrayList<>();
        deluxeDoubleRooms = new ArrayList<>();
    }

    public void addRoom(Room room) {
        if (room instanceof SingleRoom) {
            luxurySingleRooms.add((SingleRoom) room);
        } else if (room instanceof DoubleRoom) {
            luxuryDoubleRooms.add((DoubleRoom) room);
        }
    }

    public boolean isRoomAvailable(String roomType) {
        switch (roomType) {
            case "Luxury Single":
                return !luxurySingleRooms.isEmpty();
            case "Deluxe Single":
                return !deluxeSingleRooms.isEmpty();
            case "Luxury Double":
                return !luxuryDoubleRooms.isEmpty();
            case "Deluxe Double":
                return !deluxeDoubleRooms.isEmpty();
            default:
                return false;
        }
    }
}

public class Hotel {

    private static Holder holder = new Holder();

    public static void bookRoom(String roomType, String name, String contact, String gender) throws NotAvailable {
        if (!holder.isRoomAvailable(roomType)) {
            throw new NotAvailable("Room is not available.");
        }
        // Logic to book a room and add to the holder
        if (roomType.equals("Luxury Single")) {
            holder.addRoom(new SingleRoom(name, contact, gender));
        }
        // Add logic for other room types
    }

    public static String features(String roomType) {
        switch (roomType) {
            case "Luxury Single":
                return "Luxury Single Room features: King-size bed, Sea view, Air conditioning.";
            case "Deluxe Single":
                return "Deluxe Single Room features: Queen-size bed, Mountain view, Air conditioning.";
            default:
                return "Unknown room type.";
        }
    }

    public static void availability(String roomType) {
        if (holder.isRoomAvailable(roomType)) {
            System.out.println(roomType + " is available.");
        } else {
            System.out.println(roomType + " is not available.");
        }
    }

    public static double bill(Room room) {
        double totalBill = 0;
        for (Food food : room.getFoodItems()) {
            totalBill += food.getPrice() * food.getQuantity();
        }
        return totalBill;
    }

    public static void deallocate(Room room) {
        // Logic to deallocate room
    }

    public static void order(Room room, Food food) {
        room.addFood(food);
    }

    public static void display(Room room) {
        room.displayDetails();
    }
}

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.*;
import javafx.scene.layout.VBox;
import javafx.stage.Stage;

public class Main extends Application {

    @Override
    public void start(Stage primaryStage) {
        primaryStage.setTitle("Hotel Management System");

        TabPane tabPane = new TabPane();
        Tab bookingTab = new Tab("Booking Room");
        Tab availabilityTab = new Tab("Check Availability");
        Tab billTab = new Tab("Generate Bill");

        bookingTab.setContent(new Label("Room Booking Functionality"));
        availabilityTab.setContent(new Label("Availability Functionality"));
        billTab.setContent(new Label("Bill Generation Functionality"));

        tabPane.getTabs().addAll(bookingTab, availabilityTab, billTab);

        VBox root = new VBox(tabPane);
        Scene scene = new Scene(root, 400, 300);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}


