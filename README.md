using System;
using System.Collections.Generic;
using System.Windows.Forms;

public partial class AirportForm : Form
{
    private Dictionary<int, FlightDetails> flightsByNumber;
    private Dictionary<string, FlightDetails> flightsByCode;

    public AirportForm()
    {
        InitializeComponent();
        InitializeFlightData();
    }

    private void InitializeFlightData()
    {
        // Initialize flight data (replace this with your actual data)
        flightsByNumber = new Dictionary<int, FlightDetails>
        {
            { 201, new FlightDetails("AUS", "Austin", "0710") },
            { 321, new FlightDetails("CRP", "Corpus Christi", "0830") },
            { 510, new FlightDetails("DFW", "Dallas Fort Worth", "0915") },
            { 633, new FlightDetails("HOU", "Houston", "1140") }
        };

        flightsByCode = new Dictionary<string, FlightDetails>
        {
            { "AUS", new FlightDetails(201, "Austin", "0710") },
            { "CRP", new FlightDetails(321, "Corpus Christi", "0830") },
            { "DFW", new FlightDetails(510, "Dallas Fort Worth", "0915") },
            { "HOU", new FlightDetails(633, "Houston", "1140") }
        };
    }

    private void btnGetFlightInfo_Click(object sender, EventArgs e)
    {
        string userInput = txtUserInput.Text.Trim();

        if (int.TryParse(userInput, out int flightNumber))
        {
            DisplayFlightInfo(GetFlightInfo(flightNumber));
        }
        else
        {
            DisplayFlightInfo(GetFlightInfo(userInput));
        }
    }

    private string GetFlightInfo(int flightNumber)
    {
        if (flightsByNumber.ContainsKey(flightNumber))
        {
            FlightDetails flight = flightsByNumber[flightNumber];
            return $"Flight {flightNumber} from {flight.AirportCode} ({flight.AirportName}) at {flight.Time}.";
        }
        else
        {
            return "Flight not found.";
        }
    }

    private string GetFlightInfo(string airportCode)
    {
        if (flightsByCode.ContainsKey(airportCode))
        {
            FlightDetails flight = flightsByCode[airportCode];
            return $"Flight from {flight.AirportCode} ({flight.AirportName}) at {flight.Time}.";
        }
        else
        {
            return "Flight not found.";
        }
    }

    private void DisplayFlightInfo(string message)
    {
        lblFlightInfo.Text = message;
    }
}

public class FlightDetails
{
    public int FlightNumber { get; }
    public string AirportCode { get; }
    public string AirportName { get; }
    public string Time { get; }

    public FlightDetails(int flightNumber, string airportCode, string time)
    {
        FlightNumber = flightNumber;
        AirportCode = airportCode;
        AirportName = GetAirportName(airportCode);
        Time = time;
    }

    private string GetAirportName(string airportCode)
    {
        // Replace this with your actual airport name lookup logic
        switch (airportCode)
        {
            case "AUS":
                return "Austin";
            case "CRP":
                return "Corpus Christi";
            case "DFW":
                return "Dallas Fort Worth";
            case "HOU":
                return "Houston";
            default:
                return "Unknown";
        }
    }
}
