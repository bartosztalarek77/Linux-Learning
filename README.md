<!doctype html>
<html lang="pl">
<head>
  <meta charset="utf-8">
  <title>Vagrant + VMware Fusion (Apple Silicon) — README</title>
</head>
<body style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif; color:#24292e; line-height:1.6; padding:24px;">
  <div style="max-width:900px; margin:0 auto;">
    <header style="display:flex; align-items:center; justify-content:space-between;">
      <h1 style="margin:0 0 8px 0;">🖥️ Vagrant + VMware Fusion (Apple Silicon)</h1>
      <small style="color:#6a737d">README (HTML)</small>
    </header>

    <p style="margin-top:0;">Krok po kroku: instalacja Rosetta, Vagrant, VMware Fusion Tech Preview oraz uruchomienie maszyn Ubuntu i CentOS przez Vagrant + VMware provider.</p>

    <nav style="background:#f6f8fa; border:1px solid #e1e4e8; padding:12px; border-radius:6px; margin:16px 0;">
      <strong>Spis treści</strong>
      <ul style="margin:8px 0 0 20px;">
        <li><a href="#wymagania">Wymagania</a></li>
        <li><a href="#rosseta">1. Zainstaluj Rosetta</a></li>
        <li><a href="#vagrant">2. Zainstaluj Vagrant</a></li>
        <li><a href="#konto">3. Konto Broadcom/VMware</a></li>
        <li><a href="#fusion">4–5. VMware Fusion — pobranie, instalacja, uprawnienia</a></li>
        <li><a href="#vmware-utility">6–7. vagrant-vmware-utility i plugin</a></li>
        <li><a href="#ubuntu">8–10. Ubuntu VM</a></li>
        <li><a href="#centos">11–13. CentOS VM</a></li>
        <li><a href="#troubleshooting">Rozwiązywanie problemów</a></li>
      </ul>
    </nav>

    <section id="wymagania">
      <h2>Wymagania</h2>
      <ul>
        <li>macOS na Apple Silicon</li>
        <li>Homebrew (jeśli brak: <code>/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"</code>)</li>
        <li>Konto Broadcom/VMware (https://support.broadcom.com) do pobrania VMware Fusion Tech Preview</li>
      </ul>
    </section>

    <section id="rosseta">
      <h2>1. Zainstaluj Rosetta</h2>
      <p>W terminalu:</p>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>/usr/sbin/softwareupdate --install-rosetta --agree-to-license</code></pre>
    </section>

    <section id="vagrant">
      <h2>2. Zainstaluj Vagrant</h2>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>brew install vagrant</code></pre>
    </section>

    <section id="konto">
      <h2>3. Załóż konto Broadcom / VMware</h2>
      <p>Otwórz: <a href="https://support.broadcom.com">https://support.broadcom.com</a> i zarejestruj konto, aby pobrać VMware Fusion Tech Preview.</p>
    </section>

    <section id="fusion">
      <h2>4–5. Pobierz i zainstaluj VMware Fusion (Tech Preview) i nadaj uprawnienia</h2>
      <ol>
        <li>Zaloguj się na konto Broadcom/VMware.</li>
        <li>W sekcji pobrań wybierz <strong>VMware Fusion → VMware Fusion 13 Pro for Personal Use → 13.6</strong> i pobierz.</li>
        <li>Otwórz pobrany .dmg i zainstaluj (dwukliknij ikonę i potwierdź hasłem administratora).</li>
      </ol>

      <h3>Accessibility (Dostępność)</h3>
      <ol>
        <li>Otwórz System Settings (Ustawienia systemowe).</li>
        <li>Przejdź do Privacy & Security → Accessibility.</li>
        <li>Włącz przełącznik dla VMware Fusion.</li>
      </ol>
    </section>

    <section id="vmware-utility">
      <h2>6–7. Zainstaluj vagrant-vmware-utility i plugin</h2>
      <p>W terminalu:</p>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>brew install --cask vagrant-vmware-utility
vagrant plugin install vagrant-vmware-desktop</code></pre>
      <p>Uwaga: plugin i utility mogą mieć wymagania licencyjne. Jeśli napotkasz problemy, sprawdź dokumentację pluginu i wersję Vagrant.</p>
    </section>

    <section id="ubuntu">
      <h2>8–10. Ubuntu VM</h2>
      <h3>Przygotowanie folderu</h3>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>cd
mkdir -p ~/Desktop/vms/ubuntu
cd ~/Desktop/vms/ubuntu</code></pre>

      <h3>Vagrantfile (Ubuntu)</h3>
      <p>Utwórz plik <code>Vagrantfile</code> i wklej:</p>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>Vagrant.configure("2") do |config|
  config.vm.box = "spox/ubuntu-arm"
  config.vm.box_version = "1.0.0"
  config.vm.network "private_network", ip: "192.168.56.11"

  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
  end
end</code></pre>

      <h3>Uruchamianie</h3>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>vagrant up
vagrant ssh
# w VM:
sudo -i
ip addr show
# wyjście z VM:
exit
exit
# zatrzymanie i usunięcie:
vagrant halt
vagrant destroy</code></pre>
    </section>

    <section id="centos">
      <h2>11–13. CentOS VM</h2>
      <h3>Przygotowanie folderu</h3>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>cd
mkdir -p ~/Desktop/vms/centos
cd ~/Desktop/vms/centos</code></pre>

      <h3>Vagrantfile (CentOS — przykład)</h3>
      <p>Uwaga: nazwa boxa może być inna — upewnij się, że używasz boxa kompatybilnego z architekturą ARM (aarch64).</p>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>Vagrant.configure("2") do |config|
  # Podmień poniższą nazwę boxa na właściwą, jeśli trzeba
  config.vm.box = "bandit145/centos-stream9-arm"
  config.vm.network "private_network", ip: "192.168.56.12"

  config.vm.provider "vmware_desktop" do |vmware|
    vmware.gui = true
    vmware.allowlist_verified = true
  end
end</code></pre>

      <h3>Uruchamianie</h3>
      <pre style="background:#0d1117; color:#c9d1d9; padding:12px; border-radius:6px; overflow:auto;"><code>vagrant up
vagrant ssh
# w VM:
sudo -i
ip addr show
# wyjście
exit
exit
vagrant halt
vagrant destroy</code></pre>
    </section>

    <section id="troubleshooting">
      <h2>Rozwiązywanie problemów</h2>
      <ul>
        <li>Plugin <code>vagrant-vmware-desktop</code> może wymagać konkretnej wersji Vagrant — sprawdź kompatybilność.</li>
        <li>Jeśli VMware nie startuje, upewnij się, że nadałeś uprawnienia w Privacy & Security → Accessibility oraz Full Disk Access (jeśli wymagane).</li>
        <li>Jeśli box nie jest dostępny dla ARM, wyszukaj alternatywny box na <a href="https://app.vagrantup.com/">app.vagrantup.com</a> (szukaj "arm", "aarch64" lub "arm64").</li>
      </ul>
    </section>

    <footer style="margin-top:24px; padding-top:12px; border-top:1px solid #e1e4e8; color:#6a737d;">
      <p>Plik wygenerowany automatycznie. Chcesz, żebym zrobił commit do repo? Podaj link lub uprawnienia. Potrzebujesz, żebym dopasował nazwy boxów — podaj je, to poprawię.</p>
    </footer>
  </div>
</body>
</html>
