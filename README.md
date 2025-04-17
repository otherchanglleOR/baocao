using System;

class Node
{
    public string Content;
    public Node Next;

    public Node(string content)
    {
        Content = content;
        Next = null;
    }
}

class LinkedList
{
    private Node head;

    public LinkedList()
    {
        head = null;
    }

    public void AddNote(string content)
    {
        Node newNode = new Node(content);
        if (head == null)
        {
            head = newNode;
            return;
        }

        Node current = head;
        while (current.Next != null)
            current = current.Next;

        current.Next = newNode;
    }

    public void RemoveNote(string content)
    {
        if (head == null) return;

        if (head.Content == content)
        {
            head = head.Next;
            return;
        }

        Node current = head;
        while (current.Next != null && current.Next.Content != content)
            current = current.Next;

        if (current.Next != null)
            current.Next = current.Next.Next;
    }

    public void EditNote(string oldContent, string newContent)
    {
        Node current = head;
        while (current != null)
        {
            if (current.Content == oldContent)
            {
                current.Content = newContent;
                return;
            }
            current = current.Next;
        }
    }

    public void MoveNoteTo(LinkedList otherList, string content)
    {
        if (head == null) return;

        Node current = head;
        Node prev = null;

        while (current != null && current.Content != content)
        {
            prev = current;
            current = current.Next;
        }

        if (current == null) return;

        if (prev == null)
            head = current.Next;
        else
            prev.Next = current.Next;

        current.Next = null;
        otherList.AddExistingNode(current);
    }

    private void AddExistingNode(Node node)
    {
        if (head == null)
        {
            head = node;
            return;
        }

        Node current = head;
        while (current.Next != null)
            current = current.Next;

        current.Next = node;
    }

    public void Print(string title)
    {
        Console.WriteLine($"\n📋 {title}");
        Node current = head;
        if (current == null)
        {
            Console.WriteLine("(Trống)");
            return;
        }
        while (current != null)
        {
            Console.WriteLine("- " + current.Content);
            current = current.Next;
        }
    }
}

class Program
{
    static void Main()
    {
        Console.OutputEncoding = System.Text.Encoding.UTF8;

        LinkedList todoList = new LinkedList();
        LinkedList doneList = new LinkedList();

        int choice;
        do
        {
            Console.WriteLine("\n===== MENU =====");
            Console.WriteLine("1. Thêm ghi chú");
            Console.WriteLine("2. Xoá ghi chú");
            Console.WriteLine("3. Sửa ghi chú");
            Console.WriteLine("4. Di chuyển ghi chú sang danh sách ĐÃ HOÀN THÀNH");
            Console.WriteLine("5. Hiển thị danh sách");
            Console.WriteLine("0. Thoát");
            Console.Write("Chọn chức năng: ");
            choice = int.Parse(Console.ReadLine());

            switch (choice)
            {
                case 1:
                    Console.Write("Nhập nội dung ghi chú mới: ");
                    string note = Console.ReadLine();
                    todoList.AddNote(note);
                    break;
                case 2:
                    Console.Write("Nhập nội dung ghi chú cần xoá: ");
                    string delNote = Console.ReadLine();
                    todoList.RemoveNote(delNote);
                    break;
                case 3:
                    Console.Write("Nhập nội dung cũ: ");
                    string oldNote = Console.ReadLine();
                    Console.Write("Nhập nội dung mới: ");
                    string newNote = Console.ReadLine();
                    todoList.EditNote(oldNote, newNote);
                    break;
                case 4:
                    Console.Write("Nhập nội dung ghi chú cần di chuyển: ");
                    string moveNote = Console.ReadLine();
                    todoList.MoveNoteTo(doneList, moveNote);
                    break;
                case 5:
                    todoList.Print("Danh sách TODO");
                    doneList.Print("Danh sách ĐÃ HOÀN THÀNH");
                    break;
                case 0:
                    Console.WriteLine("Thoát chương trình.");
                    break;
                default:
                    Console.WriteLine("❌ Lựa chọn không hợp lệ.");
                    break;
            }

        } while (choice != 0);
    }
}
