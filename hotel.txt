import java.util.ArrayList;
import java.util.List;
import java.util.Scanner;

// Room class to represent a hotel room
class Room {
    private int roomNumber;
    private String category;
    private double pricePerNight;
    private boolean isAvailable;

    public Room(int roomNumber, String category, double pricePerNight) {
        this.roomNumber = roomNumber;
        this.category = category;
        this.pricePerNight = pricePerNight;
        this.isAvailable = true;
    }

    public int getRoomNumber() {
        return roomNumber;
    }

    public String getCategory() {
        return category;
    }

    public double getPricePerNight() {
        return pricePerNight;
    }

    public boolean isAvailable() {
        return isAvailable;
    }

    public void setAvailable(boolean available) {
        isAvailable = available;
    }

    @Override
    public String toString() {
        return "Room{" +
                "roomNumber=" + roomNumber +
                ", category='" + category + '\'' +
                ", pricePerNight=" + pricePerNight +
                ", isAvailable=" + isAvailable +
                '}';
    }
}

// Reservation class to represent a reservation
class Reservation {
    private Room room;
    private String guestName;
    private int numberOfNights;
    private double totalCost;

    public Reservation(Room room, String guestName, int numberOfNights) {
        this.room = room;
        this.guestName = guestName;
        this.numberOfNights = numberOfNights;
        this.totalCost = room.getPricePerNight() * numberOfNights;
        room.setAvailable(false);
    }

    public Room getRoom() {
        return room;
    }

    public String getGuestName() {
        return guestName;
    }

    public int getNumberOfNights() {
        return numberOfNights;
    }

    public double getTotalCost() {
        return totalCost;
    }

    @Override
    public String toString() {
        return "Reservation{" +
                "room=" + room +
                ", guestName='" + guestName + '\'' +
                ", numberOfNights=" + numberOfNights +
                ", totalCost=" + totalCost +
                '}';
    }
}

// Hotel class to manage rooms and reservations
class Hotel {
    private List<Room> rooms;
    private List<Reservation> reservations;

    public Hotel() {
        rooms = new ArrayList<>();
        reservations = new ArrayList<>();
    }

    public void addRoom(Room room) {
        rooms.add(room);
    }

    public List<Room> searchAvailableRooms(String category) {
        List<Room> availableRooms = new ArrayList<>();
        for (Room room : rooms) {
            if (room.isAvailable() && room.getCategory().equalsIgnoreCase(category)) {
                availableRooms.add(room);
            }
        }
        return availableRooms;
    }

    public Reservation makeReservation(Room room, String guestName, int numberOfNights) {
        Reservation reservation = new Reservation(room, guestName, numberOfNights);
        reservations.add(reservation);
        return reservation;
    }

    public List<Reservation> getReservations() {
        return reservations;
    }
}

// Main class to interact with the hotel reservation system
public class Main {
    public static void main(String[] args) {
        Hotel hotel = new Hotel();
        hotel.addRoom(new Room(101, "Single", 100));
        hotel.addRoom(new Room(102, "Double", 150));
        hotel.addRoom(new Room(103, "Suite", 300));

        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("Hotel Reservation System");
            System.out.println("1. Search available rooms");
            System.out.println("2. Make a reservation");
            System.out.println("3. View reservations");
            System.out.println("4. Exit");
            System.out.print("Choose an option: ");
            int option = scanner.nextInt();
            scanner.nextLine();  // Consume newline

            switch (option) {
                case 1:
                    System.out.print("Enter room category (Single/Double/Suite): ");
                    String category = scanner.nextLine();
                    List<Room> availableRooms = hotel.searchAvailableRooms(category);
                    System.out.println("Available rooms: ");
                    for (Room room : availableRooms) {
                        System.out.println(room);
                    }
                    break;

                case 2:
                    System.out.print("Enter room number: ");
                    int roomNumber = scanner.nextInt();
                    scanner.nextLine();  // Consume newline
                    System.out.print("Enter guest name: ");
                    String guestName = scanner.nextLine();
                    System.out.print("Enter number of nights: ");
                    int numberOfNights = scanner.nextInt();

                    Room room = null;
                    for (Room r : hotel.searchAvailableRooms("")) {
                        if (r.getRoomNumber() == roomNumber) {
                            room = r;
                            break;
                        }
                    }
                    if (room != null && room.isAvailable()) {
                        Reservation reservation = hotel.makeReservation(room, guestName, numberOfNights);
                        System.out.println("Reservation successful: " + reservation);
                    } else {
                        System.out.println("Room not available.");
                    }
                    break;

                case 3:
                    System.out.println("All reservations: ");
                    for (Reservation reservation : hotel.getReservations()) {
                        System.out.println(reservation);
                    }
                    break;

                case 4:
                    System.out.println("Exiting system.");
                    scanner.close();
                    System.exit(0);
                    break;

                default:
                    System.out.println("Invalid option. Please try again.");
            }
        }
    }
}