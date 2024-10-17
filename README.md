using System;

// Denna klass representerar en parkeringsruta.
public class ParkingSpot
{
    public string VehicleInfo { get; set; } // Information om fordonet (typ och registreringsnummer).
    public DateTime ArrivalTime { get; set; } // Tidsstämpel för när fordonet parkerades.

    // Konstruktor som sätter fordonets information och aktuell tid som ankomsttid.
    public ParkingSpot(string vehicleInfo)
    {
        VehicleInfo = vehicleInfo;
        ArrivalTime = DateTime.Now;
    }
}

public class ParkingGarage
{
    // En array som representerar alla parkeringsrutor.
    private ParkingSpot[] parkingSpots = new ParkingSpot[100];

    // Parkerar ett fordon på första lediga plats eller fyller en MC-plats om möjligt.
    public void ParkVehicle(string type, string regNumber)
    {
        string vehicle = type + "#" + regNumber;
        for (int i = 0; i < parkingSpots.Length; i++)
        {// Om platsen i är tom ingen bil eller motorcykel är parkerad där
            if (parkingSpots[i] == null)
            { // Skapa en ny ParkingSpot med fordonets information och parkera den på plats i
                parkingSpots[i] = new ParkingSpot(vehicle);
                Console.WriteLine($"Fordon parkerat på platsen {i + 1}.");
              // Avsluta metoden eftersom vi har parkerat fordonet och behöver inte fortsätta loopen.
                return;
            }
            //Om fordonstypen är "MC"(motorcykel), och plats i redan innehåller en motorcykel(börjar med "MC"), men inte redan innehåller två motorcyklar
            else if (type == "MC" && parkingSpots[i].VehicleInfo.StartsWith("MC") && !parkingSpots[i].VehicleInfo.Contains(","))
            {// kontrollerar om den inte redan innehåller ett komma ,.
                parkingSpots[i].VehicleInfo += "," + regNumber;
                Console.WriteLine($"Andra motorcykeln parkerad på platsen {i + 1}.");
                return;
            }
        }
        Console.WriteLine("Inga lediga platser.");
    }

    // Flyttar ett fordon från en ruta till en annan.
    public void MoveVehicle(int fromSpot, int toSpot)
    {
        if (parkingSpots[fromSpot - 1] != null && parkingSpots[toSpot - 1] == null)
        {
            parkingSpots[toSpot - 1] = parkingSpots[fromSpot - 1];
            parkingSpots[fromSpot - 1] = null;
            Console.WriteLine($"Fordonet har flyttats från platsen {fromSpot} till plats {toSpot}.");
        }
        else
        {
            Console.WriteLine("Ogiltig flytt.");
        }
    }

    // Tar bort ett fordon från en ruta och beräknar hur länge det varit parkerat.
    public void RemoveVehicle(string regNumber)
    {
        for (int i = 0; i < parkingSpots.Length; i++)
        { // Här kontrollerar vi två saker:
          //Om plats i inte är tom && Om fordonets information på plats i innehåller registreringsnumret
            if (parkingSpots[i] != null && parkingSpots[i].VehicleInfo.Contains(regNumber))
            {
                var parkingDuration = DateTime.Now - parkingSpots[i].ArrivalTime;

              //  Vi tar bort fordonet genom att sätta platsen i till null.
                parkingSpots[i] = null;
                Console.WriteLine($"Vehicle {regNumber} removed from spot {i + 1}. Parked for {parkingDuration.TotalMinutes} minutes.");
                return;
            }
        }
        Console.WriteLine("Fordonet hittades inte.");
    }

    // Söker efter ett fordon med ett specifikt registreringsnummer.
    public void SearchVehicle(string regNumber)
    {
        for (int i = 0; i < parkingSpots.Length; i++)
        {
            if (parkingSpots[i] != null && parkingSpots[i].VehicleInfo.Contains(regNumber))
            {
                Console.WriteLine($"Vehicle {regNumber} found at spot {i + 1}.");
                return;
            }
        }
        Console.WriteLine("Fordonet hittades inte.");
    }

    // Skriver ut statusen för alla parkeringsrutor.
    public void PrintParkingSpots()
    {
        for (int i = 0; i < parkingSpots.Length; i++)
        {
            if (parkingSpots[i] != null)
            {
                Console.WriteLine($"Spot {i + 1}: {parkingSpots[i].VehicleInfo}, Arrived: {parkingSpots[i].ArrivalTime}");
            }
            else
            {
                Console.WriteLine($"Spot {i + 1}: Empty");
            }
        }
    }

    // Skriver ut en sammanfattningsrapport över parkeringsplatsens status.
    public void PrintSummaryReport()
    {
        int emptySpots = 0, halfFullSpots = 0, fullSpots = 0;

        foreach (var spot in parkingSpots)
        {
            if (spot == null)
            {
                emptySpots++;
            }
        // Om platsen inte är tom, kontrollerar vi om den innehåller en , (komma).
       // Detta innebär att platsen innehåller två motorcyklar och är därmed full.
       // Om villkoret är sant, ökar vi räknaren för fulla platser
            else if (spot.VehicleInfo.Contains(","))
            {
                fullSpots++;
            }
            else
            {
                halfFullSpots++;
            }
        }

        Console.WriteLine($"Sammanfattande rapport:\nTomma platser: {emptySpots}\nHalvfulla platser: {halfFullSpots}\nFulla platser: {fullSpots}");
    }
}

class Program
{
    static void Main()
    {
        ParkingGarage garage = new ParkingGarage();

        bool running = true;
        while (running)
        {
            // Visar en meny med olika valmöjligheter.
            Console.WriteLine("1. parkera fordon\n2. Flytta fordon\n3. Ta bort fordon\n4. Sök fordon\n5. Skriv ut parkeringsplatser\n6. Skriv ut sammanfattande rapport\n7. Utgång");
            Console.Write("Välj ett alternativ: ");
            string choice = Console.ReadLine();

            switch (choice)
            {
                case "1":
                    // Användaren anger typ och registreringsnummer för fordonet som ska parkeras.
                    Console.Write("Ange fordonstyp (CAR/MC): ");
                    string type = Console.ReadLine();
                    Console.Write("Ange registreringsnummer : ");
                    string regNumber = Console.ReadLine();
                    garage.ParkVehicle(type, regNumber);
                    break;

                case "2":
                    // Användaren anger från vilken plats fordonet ska flyttas och till vilken plats.
                    Console.Write("ange från plats: ");
                    int fromSpot = int.Parse(Console.ReadLine());
                    Console.Write("ange till plats: ");
                    int toSpot = int.Parse(Console.ReadLine());
                    garage.MoveVehicle(fromSpot, toSpot);
                    break;

                case "3":
                    // Användaren anger registreringsnumret för fordonet som ska tas bort.
                    Console.Write("Ange registreringsnummer: ");
                    regNumber = Console.ReadLine();
                    garage.RemoveVehicle(regNumber);
                    break;

                case "4":
                    // Användaren anger registreringsnumret för fordonet som ska sökas.
                    Console.Write("Ange registreringsnummer: ");
                    regNumber = Console.ReadLine();
                    garage.SearchVehicle(regNumber);
                    break;

                case "5":
                    // Skriver ut statusen för alla parkeringsrutor.
                    garage.PrintParkingSpots();
                    break;

                case "6":
                    // Skriver ut en sammanfattningsrapport över parkeringsplatsens status.
                    garage.PrintSummaryReport();
                    break;

                case "7":
                    // Avslutar programmet.
                    running = false;
                    break;

                default:
                    // Felaktigt menyval.
                    Console.WriteLine("Ogiltigt alternativ, försök igen.");
                    break;
            }
        }
    }
}

