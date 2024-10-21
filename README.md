<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Collection Entries</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            margin: 0;
            padding: 20px;
        }
        h2 {
            text-align: center;
            color: #333;
            margin-bottom: 20px;
        }
        form {
            background-color: #fff;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
            margin-bottom: 20px;
        }
        label {
            display: block;
            margin: 10px 0 5px;
            color: #555;
        }
        input[type="date"], select {
            width: calc(100% - 20px);
            padding: 10px;
            margin-bottom: 15px;
            border: 1px solid #ccc;
            border-radius: 4px;
            font-size: 16px;
        }
        button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 10px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            transition: background-color 0.3s ease;
        }
        button:hover {
            background-color: #218838;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 12px;
            text-align: left;
        }
        th {
            background-color: #007bff;
            color: white;
        }
        tr:nth-child(even) {
            background-color: #f9f9f9;
        }
        tr:hover {
            background-color: #f1f1f1;
        }
        .container {
            max-width: 800px;
            margin: auto;
        }
    </style>
</head>
<body>
    <div class="container">
        <h2>Search Collection Entries</h2>
        <form id="searchForm">
            <label for="searchDate">Search by Date:</label>
            <input type="date" id="searchDate" name="searchDate">

            <label for="searchRoute">Search by Route:</label>
            <select id="searchRoute" name="searchRoute">
                <option value="">Select Route</option>
                <option value="NIMON">NIMON</option>
                <option value="SHINGVE">SHINGVE</option>
                <option value="ASHWI">ASHWI</option>
                <option value="WALKI">WALKI</option>
                <option value="CHAMBHURDI">CHAMBHURDI</option>
                <option value="AKOLNER">AKOLNER</option>
                <option value="YELPANE">YELPANE</option>
                <option value="SAMNAPUR">SAMNAPUR</option>
                <option value="BHOKAR">BHOKAR</option>
                <option value="MATHACHIWADI">MATHACHIWADI</option>
                <option value="RALEGAN">RALEGAN</option>
                <option value="DAPUR">DAPUR</option>
                <option value="PAREGAON">PAREGAON</option>
                <option value="BHENDALI">BHENDALI</option>
                <option value="BHIMATALKI">BHIMATALKI</option>
                <option value="KANHUR PATHAR">KANHUR PATHAR</option>
            </select>

            <label for="searchBMC">Select BMC:</label>
            <select id="searchBMC" name="searchBMC" disabled>
                <option value="">Select BMC</option>
                <!-- BMC options will be populated here -->
            </select>
            
            <button type="submit">Search</button>
        </form>
        
        <table>
            <thead>
                <tr>
                    <th>Date</th>
                    <th>Chemist</th>
                    <th>Route</th>
                    <th>BMC</th>
                    <th>Quantity (Liters)</th>
                    <th>AB Result</th>
                    <th>Temperature</th>
                    <th>CAMP</th>
                    <th>Reject Qty</th>
                </tr>
            </thead>
            <tbody id="collectionEntries">
                <!-- Dynamic collection entries will be added here -->
            </tbody>
        </table>
    </div>
    
    <script>
        const bmcData = {
            NIMON: ["NIMON", "MANGALAPUR", "KOKANGAON", "NIMGAON JALI", "NIMGAON PAGA"],
            SHINGVE: ["WAKADI", "SHINGVE", "EKRUKHE MHVTR", "SHIRDI MHVTR", "RAHATA"],
            ASHWI: ["NIRMAL PIMPRI", "ASHWI BDK", "ASHWI KRD", "OZAR BDK", "SHEDGAON"],
            WALKI: ["SHIRDHON", "WALKI", "GUNDEGAON", "KOREGAON", "GHOSPURI"],
            CHAMBHURDI: ["KADUS", "CHAMBHURDI", "PIMPALGAON PISA MHVTR", "KHARATWADI", "BELWANDI MHVTR"],
            AKOLNER: ["BHOYARE PATHAR", "RUI CHATRAPATI", "BABURDI", "CHAS", "AKOLNER"],
            YELPANE: ["YELAPANE", "PIMPRI KOLANDAR", "DHAVALGAON"],
            SAMNAPUR: ["SAMNAPUR", "TANPURE MALA", "PIMPARNE", "MALUNJE MHVTR"],
            BHOKAR: ["WASIMTOKA", "BELPIMPALGAON", "BHOKAR", "KHOKAR PRVTN"],
            MATHACHIWADI: ["NANDUR SHIKARI", "MATHACHIWADI", "DAHIGAON NE PRVTN", "RANJANI PRVTN", "SHIRASGAON"],
            RALEGAN: ["WADNER HAVELI PRVTN", "PARNEAR", "SIDDESHWARWADI PRVTN", "CHINCHOLI", "RALEGAN THERPAL", "KOHAKADI PRVTN"],
            DAPUR: ["VADANGALI", "KHOPDI", "KHAMBALE", "DATALI PRVTN", "MALWADI", "DAPUR"],
            PAREGAON: ["POHEGAON", "PAREGAON", "MAYGAON DEVI PRVTN", "PANCHALE"],
            BHENDALI: ["NAYGAON", "BHENDALI", "HIVARGAON", "MHALSAKHORE", "NANDUR MADHYAMESHWAR"],
            BHIMATALKI: ["NIMONE PRVTN", "PARGAON", "KASARI", "YADAVWADI", "PADALI RANJANGAON"],
            KANHUR_PATHAR: ["HINGANGAON MHVTR", "BHANDGAON", "LONI HAVELI", "KANHUR PATHAR", "WAGHWADI"]
        };

        const searchRouteSelect = document.getElementById("searchRoute");
        const searchBMCSelect = document.getElementById("searchBMC");

        searchRouteSelect.addEventListener("change", function() {
            const selectedRoute = this.value;
            searchBMCSelect.innerHTML = "<option value=''>Select BMC</option>"; // Reset BMC options
            if (selectedRoute && bmcData[selectedRoute]) {
                bmcData[selectedRoute].forEach(function(bmc) {
                    const option = document.createElement("option");
                    option.value = bmc;
                    option.textContent = bmc;
                    searchBMCSelect.appendChild(option);
                });
                searchBMCSelect.disabled = false; // Enable BMC select
            } else {
                searchBMCSelect.disabled = true; // Disable BMC select if no route is selected
            }
        });

        // Event listener for the search form
        document.getElementById("searchForm").addEventListener("submit", function(event) {
            event.preventDefault();
            const searchDate = document.getElementById("searchDate").value;
            const searchRoute = document.getElementById("searchRoute").value;
            const searchBMC = document.getElementById("searchBMC").value;

            // Here you would handle the logic to filter collection entries based on searchDate, searchRoute, and searchBMC
            // Example: fetchFilteredEntries(searchDate, searchRoute, searchBMC);
        });
    </script>
</body>
</html>
