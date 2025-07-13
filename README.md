import React, { useState } from "react";
import { MapContainer, TileLayer } from "react-leaflet";
import { Menu, MenuButton, MenuItem } from "@/components/ui/menu";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { CameraIcon, MenuIcon } from "lucide-react";

export default function VMTApp() {
  const [section, setSection] = useState("home");

  const renderMap = () => (
    <MapContainer center={[0, 0]} zoom={13} className="h-60 w-full">
      <TileLayer
        attribution='&copy; OpenStreetMap contributors'
        url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
      />
    </MapContainer>
  );

  const renderHome = () => (
    <div className="p-4 space-y-4">
      <div className="text-4xl font-bold text-center">VMT</div>
      <div className="flex flex-col md:flex-row items-center justify-between">
        <div className="flex flex-col items-center space-y-2">
          <img src="/police-light.png" alt="Alarma" className="w-16 h-16" />
          <Button>Alarma</Button>
        </div>
        <div className="w-full md:w-2/3 mt-4 md:mt-0">{renderMap()}</div>
      </div>
    </div>
  );

  const renderApoyo = () => (
    <div className="p-4 space-y-4">
      {[{
        img: "/ambulancia.png",
        label: "Ambulancia",
        number: "106"
      }, {
        img: "/bomberos.png",
        label: "Bomberos",
        number: "116"
      }, {
        img: "/policia.png",
        label: "Policía",
        number: "01 4961935"
      }].map(({ img, label, number }) => (
        <Card key={label} className="flex items-center space-x-4 p-4">
          <img src={img} alt={label} className="w-16 h-16" />
          <div>
            <div className="font-bold text-lg">{label}</div>
            <div className="text-gray-600">{number}</div>
          </div>
        </Card>
      ))}
    </div>
  );

  const renderEvacuacion = () => <div className="p-4">{renderMap()}</div>;

  const renderZonasPeligrosas = () => <div className="p-4">{renderMap()}</div>;

  const renderVideosEducativos = () => (
    <div className="p-4 space-y-4">
      {["incendios", "desastres naturales", "lesiones", "desastre cerca"].map(topic => (
        <iframe
          key={topic}
          className="w-full h-48"
          src={`https://www.youtube.com/embed?listType=search&list=${encodeURIComponent(topic)}`}
          allowFullScreen
        ></iframe>
      ))}
    </div>
  );

  const renderReportar = () => (
    <div className="p-4 space-y-4">
      <div className="flex items-center space-x-2">
        <div className="w-16 h-16 bg-gray-300 rounded-full flex items-center justify-center">🙂</div>
        <div className="bg-gray-100 p-2 rounded">Incendio cerca de la avenida X</div>
      </div>
      <div>
        <div className="font-bold text-lg mb-2">Reportes</div>
        <ul className="list-disc pl-5">
          <li>Avenida X - Incendio</li>
          <li>Calle Y - Derrumbe</li>
        </ul>
      </div>
    </div>
  );

  const renderCuenta = () => (
    <div className="p-4 space-y-4">
      <div className="flex items-center space-x-4">
        <div className="relative w-24 h-24 rounded-full bg-gray-300">
          <button className="absolute bottom-0 right-0 p-1 bg-white rounded-full">
            <CameraIcon className="w-4 h-4" />
          </button>
        </div>
        <Input placeholder="Nombre de usuario" className="w-full" />
      </div>
    </div>
  );

  const renderContent = () => {
    switch (section) {
      case "apoyo": return renderApoyo();
      case "evacuacion": return renderEvacuacion();
      case "zonas": return renderZonasPeligrosas();
      case "videos": return renderVideosEducativos();
      case "reportar": return renderReportar();
      case "cuenta": return renderCuenta();
      default: return renderHome();
    }
  };

  return (
    <div className="min-h-screen bg-white">
      <div className="p-4 border-b flex items-center justify-between">
        <Menu>
          <MenuButton as={Button} variant="ghost">
            <MenuIcon />
          </MenuButton>
          <div className="absolute mt-2 w-56 bg-white shadow-md rounded-lg p-2 z-10">
            {[{ label: "Apoyo", key: "apoyo" },
              { label: "Evacuación", key: "evacuacion" },
              { label: "Zonas peligrosas", key: "zonas" },
              { label: "Videos educativos", key: "videos" },
              { label: "Reportar", key: "reportar" },
              { label: "Cuenta", key: "cuenta" }
            ].map(item => (
              <MenuItem key={item.key} onClick={() => setSection(item.key)}>
                {item.label}
              </MenuItem>
            ))}
          </div>
        </Menu>
        <div className="font-bold text-xl">VMT</div>
      </div>
      {renderContent()}
    </div>
  );
} 
