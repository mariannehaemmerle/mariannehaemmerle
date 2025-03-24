import React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { motion, AnimatePresence } from "framer-motion";
import { Trash2, PlusCircle, Rocket } from "lucide-react";

const GLAUBENSSAETZE_KATEGORIEN = {
  "Network Marketing": {
    areal: "Kommunikation & Beziehungspflege",
    negativ: [
      "Ich wirke aufdringlich",
      "Niemand interessiert sich für mein Business",
      "Ich bin nicht überzeugend genug"
    ],
    positiv: [
      "Ich inspiriere Menschen durch mein Vorbild",
      "Mein Business zieht die richtigen Partner an",
      "Ich kommuniziere klar, wertschätzend und erfolgreich"
    ]
  },
  "Organe & Emotionen": {
    areal: "Vegetatives Nervensystem / Körperresonanz",
    negativ: [
      "Ich kann meinen Körper nicht fühlen",
      "Meine Organe sind geschwächt",
      "Emotionen blockieren meinen Energiefluss"
    ],
    positiv: [
      "Ich bin verbunden mit meinem inneren Erleben",
      "Meine Organe arbeiten im Einklang",
      "Ich fühle und löse, was in mir ist"
    ]
  },
  "Business & Erfolg": {
    areal: "Ziel- und Handlungssystem / Planung",
    negativ: [
      "Ich weiß nicht, wo ich anfangen soll",
      "Ich bin nicht gut genug für Erfolg",
      "Ich verliere schnell die Motivation"
    ],
    positiv: [
      "Ich handle klar und zielgerichtet",
      "Ich verdiene Erfolg und lebe ihn",
      "Ich bringe meine Vision in die Welt"
    ]
  },
  "Spiritualität": {
    areal: "Pinealdrüse / Höheres Selbst",
    negativ: [
      "Ich bin vom Universum getrennt",
      "Ich spüre meine innere Führung nicht",
      "Ich zweifle an meinem spirituellen Weg"
    ],
    positiv: [
      "Ich bin verbunden mit dem Göttlichen in mir",
      "Ich empfange Klarheit und Impulse",
      "Ich folge meiner Intuition und inneren Wahrheit"
    ]
  },
  "Beziehungen": {
    areal: "Limbisches System / Verbindung",
    negativ: [
      "Ich werde nicht verstanden",
      "Ich gerate immer an die falschen Menschen",
      "Ich darf keine Nähe zulassen"
    ],
    positiv: [
      "Ich erlaube mir tiefe und nährende Verbindungen",
      "Ich werde gesehen und geschätzt, so wie ich bin",
      "Ich bin offen für liebevolle Beziehungen"
    ]
  },
  "Geld & Fülle": {
    areal: "Überlebenszentrum / Wertsystem",
    negativ: [
      "Ich habe nie genug Geld",
      "Geld ist schwer zu bekommen",
      "Ich habe Angst vor Mangel"
    ],
    positiv: [
      "Ich öffne mich für Fülle in allen Lebensbereichen",
      "Geld fließt leicht und liebevoll in mein Leben",
      "Ich bin ein Magnet für Reichtum und Wertschätzung"
    ]
  },
  "Meridiane": {
    areal: "Energetisches Netzwerk / Ausgleich",
    negativ: [
      "Meine Energie ist blockiert",
      "Ich fühle mich unausgeglichen",
      "Ich bin ständig erschöpft"
    ],
    positiv: [
      "Meine Meridiane sind im Fluss",
      "Ich spüre harmonische Energie in meinem Körper",
      "Mein Energiesystem ist lebendig und klar"
    ]
  },
  "Chakren": {
    areal: "Energetische Zentren / Bewusstseinsebenen",
    negativ: [
      "Meine Chakren sind blockiert",
      "Ich fühle mich zerrissen",
      "Ich bin nicht in meiner Mitte"
    ],
    positiv: [
      "Meine Chakren sind offen und ausbalanciert",
      "Ich bin verbunden mit allen Ebenen meines Seins",
      "Ich lebe in Einklang mit meiner inneren Wahrheit"
    ]
  },
  "Abnehmen": {
    areal: "Hypothalamus / Stoffwechselzentrum",
    negativ: [
      "Ich nehme einfach nicht ab",
      "Essen kontrolliert mein Leben",
      "Mein Körper speichert alles als Fett"
    ],
    positiv: [
      "Ich nehme leicht und natürlich ab",
      "Ich esse achtsam und liebevoll",
      "Mein Körper findet sein Wohlfühlgewicht mit Leichtigkeit"
    ]
  },
  "Marketing": {
    areal: "Kommunikationszentrum / Ausdruck",
    negativ: [
      "Ich kann mein Angebot nicht klar kommunizieren",
      "Ich wirke nicht professionell genug",
      "Marketing fühlt sich für mich manipulativ an"
    ],
    positiv: [
      "Ich kommuniziere klar und mit Herz",
      "Mein Marketing ist authentisch und wirkungsvoll",
      "Ich erreiche genau die Menschen, die ich anziehen möchte"
    ]
  },
  "Kundensog": {
    areal: "Magnetisches Feld / Ausstrahlung",
    negativ: [
      "Ich muss Kunden hinterherlaufen",
      "Niemand interessiert sich für mein Angebot",
      "Ich bin unsichtbar am Markt"
    ],
    positiv: [
      "Ich ziehe genau die richtigen Kunden an",
      "Meine Energie wirkt magnetisch",
      "Kunden finden mich mit Leichtigkeit"
    ]
  },
  "Content-Magie": {
    areal: "Kreativzentrum / Ausdruckskraft",
    negativ: [
      "Ich weiß nicht, was ich posten soll",
      "Niemand interessiert sich für meine Inhalte",
      "Ich bin nicht kreativ genug"
    ],
    positiv: [
      "Meine Inhalte sind inspirierend und kraftvoll",
      "Ich bringe meine Botschaft auf magische Weise in die Welt",
      "Meine Kreativität fließt leicht und begeistert meine Community"
    ]
  },
  "Bestellung beim Universum": {
    areal: "Kreative Schöpfungszentrale",
    negativ: [
      "Ich weiß nicht, was ich will",
      "Ich bin mir meines Wertes nicht sicher",
      "Ich glaube nicht daran, dass meine Bestellung ankommt"
    ],
    positiv: [
      "Ich bestelle klar und mit Freude beim Universum",
      "Ich bin ein kraftvoller Schöpfer meiner Realität",
      "Meine Wünsche sind willkommen und dürfen erfüllt werden"
    ]
  },
  "Zellen & Reinigung": {
    areal: "Zellbewusstsein / Erneuerung",
    negativ: [
      "Meine Zellen sind überlastet und träge",
      "Ich trage alte Zellinformationen mit mir",
      "Mein Körper fühlt sich verschlackt und blockiert an"
    ],
    positiv: [
      "Meine Zellen erneuern sich in Licht und Klarheit",
      "Ich befreie mich auf Zellebene von alten Informationen",
      "Meine Zellen sind gereinigt, vital und lebendig"
    ]
  },
  "Selbstbewusstsein": {
    areal: "Frontallappen",
    negativ: [
      "Ich traue mich nicht, sichtbar zu sein",
      "Ich habe Angst vor Ablehnung",
      "Ich bin nicht überzeugend genug"
    ],
    positiv: [
      "Ich stehe zu mir und zeige mich",
      "Ich vertraue meiner Ausstrahlung",
      "Ich bin selbstbewusst und klar"
    ]
  },
  "Gesundheit": {
    areal: "Hypothalamus",
    negativ: [
      "Ich bin krank",
      "Mein Körper ist schwach",
      "Ich werde nie ganz gesund"
    ],
    positiv: [
      "Ich bin gesund und voller Energie",
      "Mein Körper regeneriert sich täglich",
      "Ich vertraue auf meine Selbstheilungskräfte"
    ]
  }
};

const TIPPS = {
  "Network Marketing": "🌐 Tipp: Sei das, was du suchst – Menschen folgen authentischer Energie.",
  "Organe & Emotionen": "❤️ Tipp: Lausche deinem Körper – er spricht über Gefühle.",
  "Business & Erfolg": "🚀 Tipp: Dein Erfolg beginnt in deinem Denken – und in deinem Herzen.",
  "Spiritualität": "🌌 Tipp: Deine innere Verbindung bringt äußeren Frieden.",
  "Beziehungen": "🤝 Tipp: Verbindung beginnt mit der Beziehung zu dir selbst.",
  "Gesundheit": "🧬 Tipp: Heilung beginnt mit Vertrauen in deinen Körper.",
  "Selbstbewusstsein": "💪 Tipp: Deine Kraft liegt darin, du selbst zu sein.",
  "Geld & Fülle": "💰 Tipp: Geld folgt deiner Wertschätzung – nicht deinem Zweifel.",
  "Meridiane": "🌀 Tipp: Energie folgt der Aufmerksamkeit – bring sie in Fluss.",
  "Chakren": "🧘 Tipp: Jedes Chakra ist ein Tor zu deiner inneren Harmonie.",
  "Abnehmen": "🥦 Tipp: Iss mit Freude und Dankbarkeit – dein Körper hört zu.",
  "Marketing": "📣 Tipp: Sprich wie ein Mensch, nicht wie ein Marktschreier.",
  "Kundensog": "🧲 Tipp: Werde zur Energie, die du anziehen willst.",
  "Content-Magie": "🪄 Tipp: Deine Worte sind Magie – teile, was dich berührt.",
  "Bestellung beim Universum": "🌠 Tipp: Klarheit + Vertrauen = Empfangsmodus."
};

const ALLE_KATEGORIEN = Object.keys(GLAUBENSSAETZE_KATEGORIEN);

function getRandomItem(array) {
  return array[Math.floor(Math.random() * array.length)];
}

export default function SynapsenApp() {
  const [arealProzent, setArealProzent] = useState(0);
  const [explosionAktiv, setExplosionAktiv] = useState(false);
  const [aktiveKategorie, setAktiveKategorie] = useState(null);
  const [positive, setPositive] = useState("");
  const [tipp, setTipp] = useState("");
  const [negative, setNegative] = useState("");

  const handleNeueKategorie = () => {
    const kategorie = getRandomItem(ALLE_KATEGORIEN);
    setAktiveKategorie(kategorie);
    const daten = GLAUBENSSAETZE_KATEGORIEN[kategorie];
    setPositive(getRandomItem(daten.positiv));
    setNegative(getRandomItem(daten.negativ));
    setArealProzent(Math.floor(Math.random() * 100));
    setTipp(TIPPS[kategorie] || "");
  };

  return (
    <div className="p-6 font-sans">
      <div className="flex flex-col gap-4 items-start mb-4">
        <h1 className="text-3xl">🧠 Synapsen App</h1>
        <motion.h2
          initial={{ opacity: 0, y: -10 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.8 }}
          className="text-xl text-gray-700 border-l-4 border-yellow-400 pl-4 py-1 bg-yellow-50 rounded shadow-sm">
          <span>✨ Aktiviere deine Schöpferkraft – Erschaffe dich NEU</span>
        </motion.h2>
        <Button
          onClick={() => {
            const kategorie = getRandomItem(ALLE_KATEGORIEN);
            setAktiveKategorie(kategorie);
            const daten = GLAUBENSSAETZE_KATEGORIEN[kategorie];
            setPositive(getRandomItem(daten.positiv));
            setNegative(getRandomItem(daten.negativ));
            setArealProzent(Math.floor(Math.random() * 100));
          }}
          className="bg-blue-500 text-white px-4 py-2 rounded-md shadow hover:bg-blue-600"
        >
          🎲 Zufällige Kategorie wählen
        </Button>
        <select
          onChange={(e) => {
            const kategorie = e.target.value;
            setAktiveKategorie(kategorie);
            const daten = GLAUBENSSAETZE_KATEGORIEN[kategorie];
            setPositive(getRandomItem(daten.positiv));
            setNegative(getRandomItem(daten.negativ));
            setArealProzent(Math.floor(Math.random() * 100));
          }}
          className="px-3 py-2 border rounded-md shadow text-sm"
        >
          <option value="">Kategorie wählen…</option>
          {ALLE_KATEGORIEN.map((kat) => (
            <option key={kat} value={kat}>{kat}</option>
          ))}
        </select>
        
      </div>

      {aktiveKategorie && (
        <Card className="p-4">
          <CardContent>
            <h2 className="text-xl font-bold mb-2">{aktiveKategorie}</h2>
            <p className="text-sm text-gray-600 mb-2">
              Gehirnareal: <strong>{GLAUBENSSAETZE_KATEGORIEN[aktiveKategorie].areal}</strong>
            </p>
            <div className="text-red-700 mb-1">
              <strong>Negativ:</strong> {negative}
              <Button
                onClick={() => setNegative("")}
                className="ml-2 text-xs bg-red-100 text-red-800 hover:bg-red-200 px-2 py-1 rounded"
              >
                🗑️ Dysfunktionale Synapse löschen
              </Button>
            </div>
            <p className="text-green-700 mb-1">
              <strong>Positiv:</strong> {positive}
            </p>
            <p className="text-sm text-blue-700 mb-2">Aktivierung: <strong>{arealProzent}%</strong></p>
            {tipp && (
              <p className="text-sm text-amber-700 mb-2">{tipp}</p>
            )}
            
            <Button
              onClick={() => setArealProzent(100)}
              className="mt-2 w-full bg-blue-100 text-blue-800 hover:bg-blue-200"
            >
              Aktivierung auf 100 % setzen
            </Button>

            <Button
              onClick={() => {
                const neueExplosionStatus = !explosionAktiv;
                setExplosionAktiv(neueExplosionStatus);
                if (!explosionAktiv) {
                  const sound = new Audio("/sounds/glitter.mp3");
                  sound.play();
                }
              }}
              className="mt-2 w-full bg-yellow-300 text-yellow-900 px-6 py-2 rounded-full shadow font-bold hover:bg-yellow-400"
            >
              ✨ Schöpferkraft aktivieren
            </Button>
          </CardContent>
        </Card>
      )}

      <AnimatePresence>
        {explosionAktiv && (
          <motion.div
            className="absolute left-1/2 top-[50%] -translate-x-1/2 -translate-y-1/2 z-50 pointer-events-none"
            initial={{ scale: 0, opacity: 0 }}
            animate={{ scale: 1.5, opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.8 }}
          >
            <div className="relative w-40 h-40">
              <div className="absolute inset-0 rounded-full bg-yellow-400 shadow-2xl border-4 border-yellow-300 animate-ping" />
              {[...Array(30)].map((_, i) => (
                <motion.div
                  key={i}
                  className="absolute w-2 h-2 rounded-full"
                  style={{
                    backgroundColor: `hsl(${Math.random() * 360}, 100%, 70%)`,
                    top: '50%',
                    left: '50%',
                    transform: 'translate(-50%, -50%)'
                  }}
                  initial={{ opacity: 1, x: 0, y: 0 }}
                  animate={{
                    x: Math.cos((i / 30) * 2 * Math.PI) * 100,
                    y: Math.sin((i / 30) * 2 * Math.PI) * 100,
                    opacity: 0
                  }}
                  transition={{ duration: 1.5, delay: 0.2 }}
                />
              ))}
            </div>
          </motion.div>
        )}

        {explosionAktiv && (
          <motion.div
            className="absolute left-1/2 top-[65%] -translate-x-1/2 mt-4 text-yellow-600 text-2xl font-bold"
            initial={{ scale: 0, opacity: 0 }}
            animate={{ scale: 1.2, opacity: 1 }}
            exit={{ opacity: 0 }}
            transition={{ duration: 0.8 }}
          >
            🌀 Neue Synapse verknüpft
            <div className="text-sm text-center text-yellow-800 mt-2">Dein neuronales Netzwerk wächst.</div>
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  );
}
