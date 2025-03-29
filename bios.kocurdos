#!/bin/bash

function display_prompt() {
    echo -n "$(pwd) | KocurDOS>> "
}

function help() {
    echo "about      - Wyświetla informacje o KocurDOS"
    echo "notatnik   - Otwiera notatnik || Nie działa, użyj "knano"!"
    echo "kalkulator - Otwiera kalkulator"
    echo "tree       - Przeglądarka plików"
    echo "clear      - Czyści ekran"
    echo "date       - Wyświetla aktualną datę i godzinę"
    echo "mkdir      - Tworzy nowy katalog"
    echo "rmdir      - Usuwa pusty katalog"
    echo "ls         - Wyświetla listę plików w bieżącym katalogu"
    echo "cd         - Zmienia katalog"
    echo "copy       - Kopiuje pliki"
    echo "del        - Usuwa pliki"
    echo "rename     - Zmienia nazwę plików"
    echo "type       - Wyświetla zawartość plików tekstowych"
    echo "flash      - Aktulizuje oprogramowanie z bios.kocurdos"
    echo "knano      - Otwiera nano w KocurDOS"
    echo "help       - Wyświetla pomoc"
    echo "exit       - Wyjście z KocurDOS"
}

function about() {
    echo ""
    echo "KocurDOS"
    echo "Wersja: 1.1.2"
    echo ""
    echo "System KocurDOS jest stworzony przez Kocurowy96"
    echo "Jest on open-source czyli każdy może zobaczyć jego kod źródłowy"
    echo "System jest napisany częściowo z pomocą AI, ale też samemu"
    echo "Wrazie jakiś błędów zgłoś je na GitHubie projektu"
    echo "https://github.com/Kocurowy96/KocurDOS"
    echo ""
}

function notatnik() {
    notatka=()
    linia=1
    while true; do
        clear
        echo "Notatnik | KocurDOS"
        echo "-------------------------------------------------"
        for ((i = 1; i <= ${#notatka[@]}; i++)); do
            echo "$i. | ${notatka[$i-1]}"
        done
        for ((i = ${#notatka[@]}+1; i <= ${#notatka[@]}+10; i++)); do
            echo "$i. |"
        done
        echo "-------------------------------------------------"
        echo "!SAVE - Zapisz | !EDIT <numer linii> - Edytuj linię | !EXIT - Wyjście"
        echo -n "$linia. | "
        read line
        if [[ "$line" == "!SAVE" ]]; then
            zapisz_do_pliku_notatnik
        elif [[ "$line" =~ !EDIT\ ([0-9]+) ]]; then
            linia_do_edytowania=${BASH_REMATCH[1]}
            edytuj_linie $linia_do_edytowania
        elif [[ "$line" == "!EXIT" ]]; then
            clear
            break
        else
            notatka+=("$line")
            ((linia++))
        fi
    done
}

function edytuj_linie() {
    local linia=$1
    if (( linia > 0 && linia <= ${#notatka[@]} )); then
        echo -n "Edytuj linię $linia: "
        read nowa_tresc
        notatka[$((linia-1))]="$nowa_tresc"
    else
        echo "Linia $linia nie istnieje!"
        read -p "Naciśnij Enter, aby kontynuować..."
    fi
}

function zapisz_do_pliku_notatnik() {
    echo -n "Podaj nazwę pliku: "
    read filename
    for ((i = 0; i < ${#notatka[@]}; i++)); do
        echo "${notatka[$i]}" >> "$filename"
    done
    echo "Notatka zapisana!"
    read -p "Naciśnij Enter, aby kontynuować..."
}

function kalkulator() {
    while true; do
        clear
        echo "Kalkulator | KocurDOS"
        echo "-------------------------------------------------"
        echo "Podaj wyrażenie matematyczne (np. 3 + 4) lub EXIT, aby wyjść:"
        read expr
        if [[ "$expr" == "EXIT" ]]; then
            break
        else
            result=$(echo "$expr" | bc -l)
            echo "Wynik: $result"
            read -p "Naciśnij Enter, aby kontynuować..."
        fi
    done
}

function tree_view() {
    while true; do
        clear
        echo "Przeglądarka plików | KocurDOS"
        echo "-------------------------------------------------"
        echo "Struktura katalogów:"
        tree .
        echo "-------------------------------------------------"
        echo "Podaj nazwę pliku do odczytania lub EXIT, aby wyjść:"
        read file
        if [[ "$file" == "EXIT" ]]; then
            break
        elif [[ -f "$file" ]]; then
            cat "$file"
            read -p "Naciśnij Enter, aby kontynuować..."
        else
            echo "Plik nie istnieje!"
            read -p "Naciśnij Enter, aby kontynuować..."
        fi
    done
}

function flash_system() {
    echo "Tworzenie kopii zapasowej kocurdos.sh jako old-kocurdos.sh..."
    cp kocurdos.sh old-kocurdos.sh
    sleep 4
    echo "Kopiowanie zawartości bios.kocurdos do kocurdos.sh..."
    cp bios.kocurdos kocurdos.sh
    sleep 4
    echo "Restartowanie KocurDOS..."
    sleep 1
    exec bash kocurdos.sh
}

function change_dir() {
    echo -n "Podaj nazwę katalogu: "
    read dir_name
    if [ -d "$dir_name" ]; then
        cd "$dir_name"
        echo "Zmieniono katalog na '$dir_name'."
    else
        echo "Katalog '$dir_name' nie istnieje!"
    fi
}

function copy_file() {
    echo -n "Podaj źródłowy plik: "
    read src_file
    echo -n "Podaj docelowy plik: "
    read dest_file
    if [ -f "$src_file" ]; then
        cp "$src_file" "$dest_file"
        echo "Plik skopiowany z '$src_file' do '$dest_file'."
    else
        echo "Plik '$src_file' nie istnieje!"
    fi
}

function delete_file() {
    echo -n "Podaj nazwę pliku do usunięcia: "
    read file_name
    if [ -f "$file_name" ]; then
        rm "$file_name"
        echo "Plik '$file_name' został usunięty!"
    else
        echo "Plik '$file_name' nie istnieje!"
    fi
}

function rename_file() {
    echo -n "Podaj obecną nazwę pliku: "
    read current_name
    echo -n "Podaj nową nazwę pliku: "
    read new_name
    if [ -f "$current_name" ]; then
        mv "$current_name" "$new_name"
        echo "Plik '$current_name' został przemianowany na '$new_name'."
    else
        echo "Plik '$current_name' nie istnieje!"
    fi
}

function type_file() {
    echo -n "Podaj nazwę pliku do wyświetlenia: "
    read file_name
    if [ -f "$file_name" ]; then
        cat "$file_name"
        read -p "Naciśnij Enter, aby kontynuować..."
    else
        echo "Plik '$file_name' nie istnieje!"
        read -p "Naciśnij Enter, aby kontynuować..."
    fi
}

function knano() {
    echo -n "Podaj nazwę pliku do edycji w nano: " 
    read file_name 
    nano "$file_name" 
}

function clear_screen() {
    clear
}

function show_date() {
    date
}

function create_dir() {
    echo -n "Podaj nazwę katalogu: "
    read dir_name
    mkdir -p "$dir_name"
    echo "Katalog '$dir_name' został utworzony!"
}

function remove_dir() {
    echo -n "Podaj nazwę katalogu do usunięcia: "
    read dir_name
    if [ -d "$dir_name" ]; then
        rmdir "$dir_name"
        echo "Katalog '$dir_name' został usunięty!"
    else
        echo "Katalog '$dir_name' nie istnieje!"
    fi
}

function list_files() {
    ls -lah | grep -v "kocurdos.sh"
}

# Sprawdź, czy narzędzie tree jest zainstalowane
if ! command -v tree &> /dev/null
then
    echo "Narzędzie 'tree' nie jest zainstalowane. Zainstaluj za pomocą 'sudo apt-get install tree'."
    exit 1
fi

clear
echo "Witaj w KocurDOS! Wpisz 'help', aby zobaczyć listę komend."
while true; do
    display_prompt
    read command
    case "$command" in
        about) about ;;
        # notatnik) notatnik ;; 
        kalkulator) kalkulator ;;
        tree) tree_view ;;
        clear) clear_screen ;;
        date) show_date ;;
        mkdir) create_dir ;;
        rmdir) remove_dir ;;
        ls) list_files ;;
        cd) change_dir ;;
        copy) copy_file ;;
        del) delete_file ;;
        rename) rename_file ;;
        type) type_file ;;
        flash) flash_system ;;
        knano) knano ;;
        help) help ;;
        exit) clear; exit 0 ;;
        *) echo "Nieznana komenda. Wpisz 'help', aby zobaczyć listę komend." ;;
    esac
done

