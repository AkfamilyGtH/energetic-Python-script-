import time
from enum import Enum, auto
import random
import sys
from colorama import Fore, Back, Style, init

# Initialize colorama for cross-platform colored text
init()

class EnergyState(Enum):
    STABLE = auto()
    RISING = auto()
    CRITICAL = auto()
    OVERLOAD = auto()
    ULTRA_HYPERDRIVE = auto()
    COSMIC_ANOMALY = auto()  # 🌠 NEW MYSTERY STATE!

class EnergySystem:
    def __init__(self):
        self.energy_level = 0
        self.state = EnergyState.STABLE
        self.MAX_SAFE_LEVEL = 5
        self.WARNING_LEVEL = 3
        self.HYPERDRIVE_CHANCE = 0.1
        self.COSMIC_ANOMALY_CHANCE = 0.07  # 7% chance for weirdness
        self._shutdown_seq = 3
        self._reboot_seq = 2
        self._cosmic_events = [
            "QUANTUM FLUCTUATION DETECTED",
            "REALITY GLITCH ACTIVATED",
            "TACHYON SURGE IMMINENT",
            "DARK MATTER PARTY IN SECTOR 5"
        ]

    def _print_cosmic(self, text, color=Fore.WHITE, delay=0.03):
        """Print text with cosmic typing animation"""
        for char in text:
            sys.stdout.write(color + char + Style.RESET_ALL)
            sys.stdout.flush()
            time.sleep(delay)
        print()

    def _random_cosmic_event(self):
        """Trigger a random cosmic anomaly"""
        event = random.choice(self._cosmic_events)
        effect = random.choice([
            lambda: self._print_cosmic(f"✨ {event} ✨", Fore.MAGENTA),
            lambda: self._print_cosmic(f"🌀 {event}🌀 ", Fore.CYAN),
            lambda: self._print_cosmic(f"⚡ {event} ⚡", Fore.YELLOW)
        ])
        
        self.state = EnergyState.COSMIC_ANOMALY
        print("\n" + "■" * 50)
        effect()
        self._print_cosmic("ENERGY SYSTEMS TEMPORARILY UNSTABLE!", Fore.RED)
        
        # Random energy fluctuation
        flux = random.randint(-2, 3)
        self.energy_level = max(0, min(self.energy_level + flux, self.MAX_SAFE_LEVEL))
        
        self._print_cosmic(f"ENERGY LEVEL FLUCTUATED BY {flux}!", Fore.GREEN)
        print("■" * 50 + "\n")
        time.sleep(1.5)
        self._update_state()

    def amplify_vibe(self):
        # Cosmic anomaly random check
        if (random.random() < self.COSMIC_ANOMALY_CHANCE and 
            self.state not in [EnergyState.OVERLOAD, EnergyState.ULTRA_HYPERDRIVE]):
            self._random_cosmic_event()
            return True
            
        if self.energy_level < self.MAX_SAFE_LEVEL:
            self.energy_level += 1
            
        self._update_state()
        
        if (self.state == EnergyState.CRITICAL and 
            random.random() < self.HYPERDRIVE_CHANCE):
            self._activate_hyperdrive()
            return False
            
        self._display_status()
        
        if self.state == EnergyState.OVERLOAD:
            self._emergency_shutdown()
            return False
        return True

    def _activate_hyperdrive(self):
        """Handle the ultra rare hyperdrive event"""
        self.state = EnergyState.ULTRA_HYPERDRIVE
        self.energy_level = 999
        
        self._print_cosmic("\n💫 W A R N I N G: HYPERDRIVE INITIATED", Fore.MAGENTA)
        time.sleep(1)
        print(f"\n{Fore.YELLOW}|{'🌈⚡'*10}|{Style.RESET_ALL}")
        self._print_cosmic(f"ENERGY LEVEL: {self.energy_level} - TRANSCENDING PHYSICS...", Fore.CYAN)
        time.sleep(1.5)
        self._big_bang()

    def _display_status(self):
        """Show current system status with visual feedback"""
        visual = (Fore.YELLOW + "⚡" * min(self.energy_level, self.MAX_SAFE_LEVEL) + 
                 Fore.BLUE + "🌌" * max(0, self.MAX_SAFE_LEVEL - self.energy_level))
                
        status_msgs = {
            EnergyState.STABLE: f"|{visual}{Style.RESET_ALL}| {Fore.GREEN}System Nominal [{self.energy_level}/{self.MAX_SAFE_LEVEL}]{Style.RESET_ALL}",
            EnergyState.RISING: f"|{visual}{Style.RESET_ALL}| {Fore.YELLOW}WARNING: Hype Rising [{self.energy_level}/{self.MAX_SAFE_LEVEL}]{Style.RESET_ALL}",
            EnergyState.CRITICAL: f"|{visual}{Style.RESET_ALL}| {Fore.RED}🚨 CRITICAL [{self.energy_level}/{self.MAX_SAFE_LEVEL}]{Style.RESET_ALL}",
            EnergyState.OVERLOAD: f"|{Fore.RED}{'💥'*self.MAX_SAFE_LEVEL}{Style.RESET_ALL}| {Back.RED}OVERLOAD DETECTED{Style.RESET_ALL}",
            EnergyState.COSMIC_ANOMALY: f"|{Fore.MAGENTA}{'✨'*self.MAX_SAFE_LEVEL}{Style.RESET_ALL}| {Fore.MAGENTA}COSMIC INSTABILITY{Style.RESET_ALL}"
        }
        
        print(f"\n{status_msgs[self.state]}")

    # ... (rest of your existing methods remain the same, just add color!)

if __name__ == "__main__":
    core = EnergySystem()
    core.infinite_energy()
