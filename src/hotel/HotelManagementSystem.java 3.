package hotel;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.Map;
import java.util.Scanner;

class HotelException extends Exception {
    public HotelException(String message) {
        super(message);
    }
}

class Room {
    private int roomNumber;
    private String type;
    private double price;
    private boolean available;

    public Room(int roomNumber, String type, double price) {
        this.roomNumber = roomNumber;
        this.type = type;
        this.price = price;
        this.available = true;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public String getType() {
        return type;
    }

    public double getPrice() {
        return price;
    }

    public boolean isAvailable() {
        return available;
    }

    public void setAvailable(boolean available) {
        this.available = available;
    }

    public void display() {
        System.out.printf("Room: %d | Type: %s | Price: Rs. %.2f | Status: %s%n",
                roomNumber, type, price,
                available ? "Available" : "Occupied");
    }
}

class Booking {
    private int bookingId;
    private String guestName;
    private int roomNumber;
    private int nights;
    private boolean checkedIn;

    public Booking(int bookingId, String guestName, int roomNumber, int nights) {
        this.bookingId = bookingId;
        this.guestName = guestName;
        this.roomNumber = roomNumber;
        this.nights = nights;
        this.checkedIn = false;
    }

    public int getBookingId() {
        return bookingId;
    }

    public String getGuestName() {
        return guestName;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public int getNights() {
        return nights;
    }

    public boolean isCheckedIn() {
        return checkedIn;
    }

    public void setCheckedIn(boolean checkedIn) {
        this.checkedIn = checkedIn;
    }
}

public class HotelManagementSystem {

    private static final Scanner sc = new Scanner(System.in);

    // ArrayList stores all rooms and bookings.
    private static final ArrayList<Room> rooms = new ArrayList<Room>();
    private static final ArrayList<Booking> bookings = new ArrayList<Booking>();

    // HashMap provides quick lookup by room number and booking ID.
    private static final HashMap<Integer, Room> roomMap =
            new HashMap<Integer, Room>();
    private static final HashMap<Integer, Booking> bookingMap =
            new HashMap<Integer, Booking>();

    private static int nextBookingId = 1001;

    private static void initializeRooms() {
        addRoom(101, "Single", 1000);
        addRoom(102, "Single", 1000);
        addRoom(201, "Double", 1800);
        addRoom(202, "Double", 1800);
        addRoom(301, "Deluxe", 3000);
        addRoom(302, "Deluxe", 3000);
    }

    private static void addRoom(int number, String type, double price) {
        Room room = new Room(number, type, price);
        rooms.add(room);
        roomMap.put(number, room);
    }

    private static int readInt(String message) {
        while (true) {
            System.out.print(message);
            try {
                return Integer.parseInt(sc.nextLine().trim());
            } catch (NumberFormatException e) {
                System.out.println("Error: Please enter a valid whole number.");
            }
        }
    }

    private static String readName() {
        while (true) {
            System.out.print("Enter guest name: ");
            String name = sc.nextLine().trim();

            if (!name.isEmpty()) {
                return name;
            }

            System.out.println("Error: Guest name cannot be empty.");
        }
    }

    private static void viewAllRooms() {
        System.out.println("\n--- ALL HOTEL ROOMS ---");
        for (Room room : rooms) {
            room.display();
        }
    }

    private static void viewAvailableRooms() {
        System.out.println("\n--- AVAILABLE ROOMS ---");
        boolean found = false;

        for (Room room : rooms) {
            if (room.isAvailable()) {
                room.display();
                found = true;
            }
        }

        if (!found) {
            System.out.println("No rooms are currently available.");
        }
    }

    private static void bookRoom() throws HotelException {
        int roomNumber = readInt("Enter room number: ");
        Room room = roomMap.get(roomNumber);

        if (room == null) {
            throw new HotelException("Room does not exist.");
        }

        if (!room.isAvailable()) {
            throw new HotelException("Room is already occupied or booked.");
        }

        String guestName = readName();
        int nights;

        while (true) {
            nights = readInt("Enter number of nights: ");
            if (nights > 0 && nights <= 365) {
                break;
            }
            System.out.println("Enter a number of nights between 1 and 365.");
        }

        int id = nextBookingId++;
        Booking booking = new Booking(id, guestName, roomNumber, nights);

        bookings.add(booking);
        bookingMap.put(id, booking);
        room.setAvailable(false);

        System.out.println("\nRoom booked successfully!");
        System.out.println("Booking ID: " + id);
        System.out.println("Guest: " + guestName);
        System.out.println("Room: " + roomNumber);
        System.out.printf("Estimated room charge: Rs. %.2f%n",
                room.getPrice() * nights);
    }

    private static void searchBooking() {
        int id = readInt("Enter booking ID: ");
        Booking booking = bookingMap.get(id);

        if (booking == null) {
            System.out.println("Booking not found.");
            return;
        }

        Room room = roomMap.get(booking.getRoomNumber());

        System.out.println("\n--- BOOKING DETAILS ---");
        System.out.println("Booking ID: " + booking.getBookingId());
        System.out.println("Guest: " + booking.getGuestName());
        System.out.println("Room: " + booking.getRoomNumber());
        System.out.println("Room type: " + room.getType());
        System.out.println("Nights: " + booking.getNights());
        System.out.println("Check-in: "
                + (booking.isCheckedIn() ? "Completed" : "Pending"));
    }

    private static void checkIn() throws HotelException {
        int id = readInt("Enter booking ID for check-in: ");
        Booking booking = bookingMap.get(id);

        if (booking == null) {
            throw new HotelException("Booking not found.");
        }

        if (booking.isCheckedIn()) {
            throw new HotelException("Guest has already checked in.");
        }

        booking.setCheckedIn(true);
        System.out.println("Check-in completed for " + booking.getGuestName());
    }

    private static void checkOut() throws HotelException {
        int id = readInt("Enter booking ID for check-out: ");
        Booking booking = bookingMap.get(id);

        if (booking == null) {
            throw new HotelException("Booking not found.");
        }

        if (!booking.isCheckedIn()) {
            throw new HotelException("Guest must check in before check-out.");
        }

        Room room = roomMap.get(booking.getRoomNumber());
        double bill = room.getPrice() * booking.getNights();

        System.out.println("\n--- FINAL HOTEL BILL ---");
        System.out.println("Guest: " + booking.getGuestName());
        System.out.println("Room number: " + booking.getRoomNumber());
        System.out.println("Number of nights: " + booking.getNights());
        System.out.printf("Price per night: Rs. %.2f%n", room.getPrice());
        System.out.printf("Total bill: Rs. %.2f%n", bill);

        room.setAvailable(true);
        bookings.remove(booking);
        bookingMap.remove(id);

        System.out.println("Check-out completed. Room is now available.");
    }

    private static void showMenu() {
        System.out.println("\n===== HOTEL MANAGEMENT SYSTEM =====");
        System.out.println("1. View All Rooms");
        System.out.println("2. View Available Rooms");
        System.out.println("3. Book a Room");
        System.out.println("4. Search Booking");
        System.out.println("5. Check In");
        System.out.println("6. Check Out and Generate Bill");
        System.out.println("7. Exit");
    }

    public static void main(String[] args) {
        initializeRooms();

        while (true) {
            showMenu();
            int choice = readInt("Enter your choice: ");

            try {
                switch (choice) {
                    case 1:
                        viewAllRooms();
                        break;
                    case 2:
                        viewAvailableRooms();
                        break;
                    case 3:
                        bookRoom();
                        break;
                    case 4:
                        searchBooking();
                        break;
                    case 5:
                        checkIn();
                        break;
                    case 6:
                        checkOut();
                        break;
                    case 7:
                        System.out.println("Thank you for using the Hotel Management System!");
                        sc.close();
                        return;
                    default:
                        System.out.println("Invalid choice. Select 1-7.");
                }
            } catch (HotelException e) {
                System.out.println("Operation failed: " + e.getMessage());
            }
        }
    }
}
