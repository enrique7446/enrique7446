<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Alertas de Conferencias</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h1>Alertas de Conferencias de Economía y Contabilidad</h1>
        <button id="check-btn">Verificar Nuevas Conferencias</button>
        <div id="alerts"></div>
    </div>
    <script src="script.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 600px;
    margin: auto;
    padding: 20px;
    background: #fff;
    border-radius: 5px;
    box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
}

button {
    padding: 10px 15px;
    background-color: #007bff;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #0056b3;
}

#alerts {
    margin-top: 20px;
}
const checkButton = document.getElementById('check-btn');
const alertsDiv = document.getElementById('alerts');

// Función para verificar nuevas conferencias
const checkForNewConferences = async () => {
    try {
        const response = await fetch('https://portal.utp.edu.pe/servicios/eventos');
        const text = await response.text();
        
        // Crear un parser de HTML
        const parser = new DOMParser();
        const doc = parser.parseFromString(text, 'text/html');
        
        // Buscar conferencias
        const events = doc.querySelectorAll('.event-item'); // Cambiar este selector según la estructura real
        const economicsEvents = Array.from(events).filter(event => 
            event.textContent.includes('Economía') || event.textContent.includes('Contabilidad')
        );

        if (economicsEvents.length > 0) {
            alertsDiv.innerHTML = '<h2>Nuevas Conferencias:</h2>' + economicsEvents.map(event => `<p>${event.textContent}</p>`).join('');
        } else {
            alertsDiv.innerHTML = '<p>No hay nuevas conferencias en este momento.</p>';
        }
    } catch (error) {
        console.error('Error fetching conferences:', error);
        alertsDiv.innerHTML = '<p>Error al verificar conferencias.</p>';
    }
};

checkButton.addEventListener('click', checkForNewConferences);
