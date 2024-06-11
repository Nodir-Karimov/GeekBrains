def show_menu():
    print("\nВыберите необходимое действие:\n"
          "1. Отобразить весь справочник\n"
          "2. Найти абонента по фамилии\n"
          "3. Изменить номер телефона абонента по фамилии\n"
          "4. Удалить абонента по фамилии\n"
          "5. Найти абонента по номеру телефона\n"
          "6. Добавить абонента в справочник\n"
          "7. Закончить работу\n"
          "8. Копировать строку из одного файла в другой")
    choice = int(input())
    return choice

def read_txt(filename):
    phone_book = []
    fields = ['Фамилия', 'Имя', 'Телефон', 'Описание']

    with open(filename, 'r', encoding='utf-8') as phb:
        for line in phb:
            record = dict(zip(fields, line.strip().split(',')))
            phone_book.append(record)

    return phone_book

def write_txt(filename, phone_book):
    with open(filename, 'w', encoding='utf-8') as phout:
        for record in phone_book:
            s = ','.join(record.values())
            phout.write(f'{s}\n')

def print_result(phone_book):
    for record in phone_book:
        print(record)

def find_by_lastname(phone_book, last_name):
    return [record for record in phone_book if record['Фамилия'].strip() == last_name.strip()]

def change_number(phone_book, last_name, new_number):
    for record in phone_book:
        if record['Фамилия'].strip() == last_name.strip():
            record['Телефон'] = new_number
            return "Номер изменен."
    return "Фамилия не найдена."

def delete_by_lastname(phone_book, last_name):
    for i, record in enumerate(phone_book):
        if record['Фамилия'].strip() == last_name.strip():
            del phone_book[i]
            return "Запись удалена."
    return "Фамилия не найдена."

def find_by_number(phone_book, number):
    return [record for record in phone_book if record['Телефон'].strip() == number.strip()]

def add_user(phone_book, user_data):
    fields = ['Фамилия', 'Имя', 'Телефон', 'Описание']
    record = dict(zip(fields, user_data.split(',')))
    phone_book.append(record)

def copy_line(source_file, target_file, line_number):
    try:
        with open(source_file, 'r', encoding='utf-8') as src:
            lines = src.readlines()

        if line_number < 1 or line_number > len(lines):
            return f"Ошибка: Номер строки должен быть в диапазоне от 1 до {len(lines)}"

        line_to_copy = lines[line_number - 1]

        with open(target_file, 'a', encoding='utf-8') as tgt:
            tgt.write(line_to_copy)

        return f"Строка номер {line_number} успешно скопирована из {source_file} в {target_file}"

    except FileNotFoundError:
        return "Ошибка: Один из файлов не найден."
    except Exception as e:
        return f"Произошла ошибка: {e}"

def work_with_phonebook():
    choice = show_menu()
    phone_book = read_txt('phonebook.txt')

    while choice != 7:
        if choice == 1:
            print_result(phone_book)
        elif choice == 2:
            last_name = input('Введите фамилию: ')
            print(find_by_lastname(phone_book, last_name))
        elif choice == 3:
            last_name = input('Введите фамилию: ')
            new_number = input('Введите новый номер: ')
            print(change_number(phone_book, last_name, new_number))
        elif choice == 4:
            last_name = input('Введите фамилию: ')
            print(delete_by_lastname(phone_book, last_name))
        elif choice == 5:
            number = input('Введите номер: ')
            print(find_by_number(phone_book, number))
        elif choice == 6:
            user_data = input('Введите новые данные (Фамилия, Имя, Телефон, Описание): ')
            add_user(phone_book, user_data)
            write_txt('phonebook.txt', phone_book)
        elif choice == 8:
            source_file = input('Введите имя исходного файла: ')
            target_file = input('Введите имя целевого файла: ')
            line_number = int(input('Введите номер строки для копирования: '))
            print(copy_line(source_file, target_file, line_number))

        choice = show_menu()

if __name__ == "__main__":
    work_with_phonebook()
