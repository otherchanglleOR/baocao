using System;

class Node
{
    public string Content; public Node Next;

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

        Dictionary<string, LinkedList> lists = new Dictionary<string, LinkedList>();
        string currentListName = "";

        int choice;
        do
        {
            Console.WriteLine("\n===== MENU =====");
            Console.WriteLine("1. Tạo danh sách mới");
            Console.WriteLine("2. Chọn danh sách để làm việc");
            Console.WriteLine("3. Thêm ghi chú");
            Console.WriteLine("4. Xoá ghi chú");
            Console.WriteLine("5. Sửa ghi chú");
            Console.WriteLine("6. Di chuyển ghi chú sang danh sách khác");
            Console.WriteLine("7. Hiển thị tất cả danh sách");
            Console.WriteLine("0. Thoát");
            Console.Write("Chọn chức năng: ");

            string input = Console.ReadLine();
            if (!int.TryParse(input, out choice))
            {
                Console.WriteLine("❌ Vui lòng nhập một số hợp lệ.");
                choice = -1;
                continue;
            }

            switch (choice)
            {
                case 1:
                    Console.Write("Nhập tên danh sách mới: ");
                    string newListName = Console.ReadLine();
                    if (!lists.ContainsKey(newListName))
                    {
                        lists[newListName] = new LinkedList();
                        Console.WriteLine($"✅ Đã tạo danh sách '{newListName}'.");
                    }
                    else
                    {
                        Console.WriteLine("⚠️ Danh sách này đã tồn tại.");
                    }
                    break;

                case 2:
                    Console.Write("Nhập tên danh sách để chọn: ");
                    string listToSelect = Console.ReadLine();
                    if (lists.ContainsKey(listToSelect))
                    {
                        currentListName = listToSelect;
                        Console.WriteLine($"📌 Đang làm việc với danh sách '{currentListName}'.");
                    }
                    else
                    {
                        Console.WriteLine("❌ Danh sách không tồn tại.");
                    }
                    break;

                case 3:
                    if (!CheckListSelected(currentListName, lists)) break;
                    Console.Write("Nhập nội dung ghi chú mới: ");
                    string note = Console.ReadLine();
                    lists[currentListName].AddNote(note);
                    break;

                case 4:
                    if (!CheckListSelected(currentListName, lists)) break;
                    Console.Write("Nhập nội dung ghi chú cần xoá: ");
                    string delNote = Console.ReadLine();
                    lists[currentListName].RemoveNote(delNote);
                    break;

                case 5:
                    if (!CheckListSelected(currentListName, lists)) break;
                    Console.Write("Nhập nội dung cũ: ");
                    string oldNote = Console.ReadLine();
                    Console.Write("Nhập nội dung mới: ");
                    string newNote = Console.ReadLine();
                    lists[currentListName].EditNote(oldNote, newNote);
                    break;

                case 6:
                    if (!CheckListSelected(currentListName, lists)) break;
                    Console.Write("Nhập nội dung ghi chú cần di chuyển: ");
                    string moveNote = Console.ReadLine();
                    Console.Write("Nhập tên danh sách đích: ");
                    string targetList = Console.ReadLine();
                    if (!lists.ContainsKey(targetList))
                    {
                        Console.WriteLine("❌ Danh sách đích không tồn tại.");
                        break;
                    }
                    lists[currentListName].MoveNoteTo(lists[targetList], moveNote);
                    break;

                case 7:
                    foreach (var listName in lists.Keys)
                        lists[listName].Print($"📋 {listName}");
                    break;

                case 0:
                    Console.WriteLine("👋 Thoát chương trình.");
                    break;

                default:
                    Console.WriteLine("❌ Lựa chọn không hợp lệ.");
                    break;
            }

        } while (choice != 0);
    }

    static bool CheckListSelected(string currentList, Dictionary<string, LinkedList> lists)
    {
        if (string.IsNullOrEmpty(currentList) || !lists.ContainsKey(currentList))
        {
            Console.WriteLine("⚠️ Vui lòng chọn danh sách hợp lệ trước.");
            return false;
        }
        return true;
    }
}
