import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Button } from "@/components/ui/button";
import { motion } from "framer-motion";
import { useNavigate } from "react-router-dom";

export default function EditDocument() {
  const [country, setCountry] = useState(localStorage.getItem("mobywatel_country") || "");
  const [fullName, setFullName] = useState(localStorage.getItem("mobywatel_fullName") || "");
  const [birthDate, setBirthDate] = useState(localStorage.getItem("mobywatel_birthDate") || "");
  const [parentName, setParentName] = useState(localStorage.getItem("mobywatel_parentName") || "");
  const [citizenship, setCitizenship] = useState(localStorage.getItem("mobywatel_citizenship") || "");
  const [photo, setPhoto] = useState(localStorage.getItem("mobywatel_photo") || null);
  
  const navigate = useNavigate();

  const handleSave = () => {
    localStorage.setItem("mobywatel_country", country);
    localStorage.setItem("mobywatel_fullName", fullName);
    localStorage.setItem("mobywatel_birthDate", birthDate);
    localStorage.setItem("mobywatel_parentName", parentName);
    localStorage.setItem("mobywatel_citizenship", citizenship);
    localStorage.setItem("mobywatel_photo", photo);

    alert("Dane zapisane!");
    navigate("/");  // Po zapisaniu danych przekierowuje na stronę główną
  };

  const handlePhotoUpload = (e) => {
    const file = e.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setPhoto(reader.result);
      };
      reader.readAsDataURL(file);
    }
  };

  return (
    <div className="min-h-screen flex items-center justify-center bg-[#0e1a2b] p-4 text-white">
      <motion.div
        initial={{ opacity: 0, y: 20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.5 }}
        className="w-full max-w-xl"
      >
        <Card className="bg-[#16243b] border border-gray-700 p-6 rounded-2xl shadow-2xl">
          <CardContent className="flex flex-col gap-4">
            <h2 className="text-2xl font-bold text-center">Edycja danych osobowych</h2>
            <Input
              placeholder="Kraj pochodzenia"
              value={country}
              onChange={(e) => setCountry(e.target.value)}
              className="bg-white text-black"
            />
            <Input
              placeholder="Imię i nazwisko"
              value={fullName}
              onChange={(e) => setFullName(e.target.value)}
              className="bg-white text-black"
            />
            <Input
              type="date"
              placeholder="Data urodzenia"
              value={birthDate}
              onChange={(e) => setBirthDate(e.target.value)}
              className="bg-white text-black"
            />
            <Input
              placeholder="Nazwisko rodzica"
              value={parentName}
              onChange={(e) => setParentName(e.target.value)}
              className="bg-white text-black"
            />
            <Input
              placeholder="Obywatelstwo"
              value={citizenship}
              onChange={(e) => setCitizenship(e.target.value)}
              className="bg-white text-black"
            />
            <input
              type="file"
              accept="image/*"
              onChange={handlePhotoUpload}
              className="text-white"
            />
            {photo && <img src={photo} alt="Zdjęcie" className="w-32 h-32 object-cover rounded" />}
            <Button onClick={handleSave} className="bg-green-600 hover:bg-green-700">Zapisz dane</Button>
          </CardContent>
        </Card>
      </motion.div>
    </div>
  );
}
