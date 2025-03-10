import { useState } from "react";
import { MapContainer, TileLayer, Marker, Popup } from "react-leaflet";
import "leaflet/dist/leaflet.css";
import { Input, Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Loader } from "lucide-react";

export default function ChalandoVelo() {
  const [search, setSearch] = useState("");
  const [loading, setLoading] = useState(false);
  const [locations, setLocations] = useState([]);

  const handleSearch = async () => {
    setLoading(true);
    // Simulated API call - replace with actual backend logic
    setTimeout(() => {
      setLocations([
        { id: 1, name: "Magasin A", lat: 45.75, lon: 4.85, score: 80 },
        { id: 2, name: "Magasin B", lat: 45.76, lon: 4.86, score: 65 },
      ]);
      setLoading(false);
    }, 2000);
  };

  return (
    <div className="p-4">
      <h1 className="text-xl font-bold mb-4">Chalando Vélo - Zone de Chalandise</h1>
      <div className="flex gap-2 mb-4">
        <Input
          type="text"
          placeholder="Rechercher une ville ou un département"
          value={search}
          onChange={(e) => setSearch(e.target.value)}
        />
        <Button onClick={handleSearch} disabled={loading}>
          {loading ? <Loader className="animate-spin" /> : "Rechercher"}
        </Button>
      </div>
      <Card>
        <CardContent>
          <MapContainer center={[45.75, 4.85]} zoom={12} className="h-96 w-full">
            <TileLayer
              url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
              attribution='&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors'
            />
            {locations.map((loc) => (
              <Marker key={loc.id} position={[loc.lat, loc.lon]}>
                <Popup>
                  <strong>{loc.name}</strong>
                  <br /> Score de potentiel : {loc.score}%
                </Popup>
              </Marker>
            ))}
          </MapContainer>
        </CardContent>
      </Card>
    </div>
  );
}
