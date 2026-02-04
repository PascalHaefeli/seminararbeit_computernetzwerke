
---
---

03022026
	Zusammenfassung
		DHCP funktioniert jetzt mit allen Geräten, alle Geräte können sich gegenseitig pingen und die Switches waren vorher Layer 2 Switches, wir müssen aber Layer 3 Switches verwenden, also habe ich diese ausgetauscht. Details folgen...
	- Layer 2 Switches durch Layer 3 Switches ersetzt (weil Aufgabenstellung - Unterschied: Layer 3 hat IPs, Layer 2 nicht)
		- Es kann immer nur das Interface des VLANs gepingt werden (192.168.10.1 für VLAN10, ... 20.1 für VLAN20 und ... 30.1 für VLAN30 bei Switch0 im 1.Stock; dieselben Adressen mit Endung auf ... .2 für Switch1 im EG, siehe Notizen in .pkt)
		- DHCP vergibt nur den anderen Servern IPs (wahrscheinlich weil diese im selben VLAN sind, wie der DHCP und die Switches, die ich einfach mal dem VLAN für die Server, also VLAN 30, zugewiesen haben)
		- DHCP direkt mit dem Switch verbunden statt via den Router (hat mein Bruder, Informatiker, mir so geraten)
		- Ich habe den Interfaces für VLANs 10 und 20 auf Switch0 und Switch1 IP Helper-Adressen gegeben (beide sind 192.168.30.10, die des DHCP Servers), damit sie den DHCP erreichen können (leitet DHCP Discovers und -Requests automatisch auf die angegebene Addresse weiter, glaube ich). VLAN 30 braucht keine Helper-Address, da es im selben VLAN ist, wie der DHCP, somit wird der auch so via BC gefunden. 
			Wir könnten auch statt dem Server den Switch als DHCP Server verwenden, aber in der Aufgabenstellung steht explizit, wir sollen dafür einen Server nehmen (sprich nicht den L3 Switch, der das auch könnte, dann hätten wir dieses Problem nicht).
		- Die Switches haben die IPs 192.168.30.20 (Switch0, 1.Stock) und 192.168.30.30 (Switch1, EG)
		- Die Verbindungen wurden grösstenteils über dieselben Ports verkabelt wie bei der Version vom 1.2. (von Näthi); Änderungen: Switches0 und 1 sind jetzt via Gig0/2 zu Gig0/2 verkabelt; DHCP Server0 ist jetzt via Fa0/5 mit Switch0 verkabelt

04022026
	added etherchannel in two separate ways; in the base file, I used ports fa23-24 and gig0/2 as trunk (unneccessary) and in the new file from 04022026 i used fa21-22, instead, where i did not have remnants of l3 config that prevented me from trunking (couldn't get rid of them, so i used different ports)

To-Do:
    - Die Portbelegung, für die ich mich entscheide, in der Dokumentation updaten.
	- Scalability
		Aktuell haben wir die Ports 1-4 bei den Switches jeweils einem VLAN zugewiesen, Port 0/5 ist bei Switch0 für den DHCP; die anderen Ports sind nicht zugewiesen. Wir müssten die Ports jeweils so zuweisen, dass weitere Geräte verbunden werden könnten (dafür haben wir zu wenige Ports auf einem Switch - wir haben 24, es sollten aber 100 Geräte unterstützt werden)
	- Resilience
		Wie schaffen wir es, dass das System bei einem Stromausfall noch funktioniert? Wir können ja keinen Notgenerator im PT verwenden, oder?
	- Dokumentation
		Wir müssen unsere Entscheidungen noch beschreiben und begründen, sowie alles erklären, das nicht funktioniert, was das Problem sein könnte und was wir versucht haben (Teilpunkte).
