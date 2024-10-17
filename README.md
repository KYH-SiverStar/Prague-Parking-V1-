    bool running = true;
    
    



    while (running)
    {
        // NY MENY MED CLEAR
        Console.Clear();
        Console.WriteLine(); // Luft
        Console.WriteLine("Valet Parking");
        Console.WriteLine("*************");
        Console.WriteLine(); // Luft
        Console.WriteLine("Menu:");
        Console.WriteLine(); // Luft

        // Menyval
        Console.WriteLine("1. Lägg till fordon");
        Console.WriteLine("2. Ta bort fordon");
        Console.WriteLine("3. Flytta fordon");
        Console.WriteLine("4. Sök efter fordon");
        Console.WriteLine("5. Visa hela parkeringsgaraget");
        Console.WriteLine("6. Skriv ut sammanfattande rapport");
        Console.WriteLine("7. Avsluta programmet");
        Console.WriteLine(); // Luft
        Console.Write("Välj ett alternativ: ");
        string option = Console.ReadLine(); // Tar in valet





        switch (option)
        {
            case "1":
                // Användaren anger typ och registreringsnummer för fordonet som ska parkeras.
                Console.Write("Ange fordonstyp (CAR/MC): ");
                string type = Console.ReadLine().ToUpper();
                Console.Write("Ange registreringsnummer : ");
                string regNumber = Console.ReadLine();
                garage.ParkVehicle(type, regNumber);
                break;

            case "2":
                // Användaren anger registreringsnumret för fordonet som ska tas bort.
                Console.Write("Ange registreringsnummer: ");
                regNumber = Console.ReadLine();
                garage.RemoveVehicle(regNumber);
                break;

            case "3":
                // Användaren anger från vilken plats fordonet ska flyttas och till vilken plats.
                Console.Write("Ange från plats: ");
                if (int.TryParse(Console.ReadLine(), out int fromSpot) && fromSpot > 0 && fromSpot <= 100)
                {
                    Console.Write("Ange till plats: ");
                    if (int.TryParse(Console.ReadLine(), out int toSpot) && toSpot > 0 && toSpot <= 100)
                    {
                        garage.MoveVehicle(fromSpot, toSpot);
                    }
                    else
                    {
                        Console.WriteLine("Ogiltig plats.");
                    }
                }
                else
                {
                    Console.WriteLine("Ogiltig plats.");
                }
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

        Console.WriteLine("Tryck på valfri tangent för att fortsätta...");
        Console.ReadKey();
    }
}
