---
title: "Hello"
date: 2018-10-25T12:38:25+11:00
draft: true
---


Xamarin Forms bring a lot of flexiblity in terms of the mobile development for both iOS and Android platforms.

```c#
public class Book
{
    public string BookName { get; set; }
}
```

Let's feed a list of books with random data:

```c#
ObservableCollection<Employee> employees = new ObservableCollection<Employee>();

public FirstListViewSppPage()
{
    InitializeComponent();


    books.Add(new Book { DisplayName = "Rob Finnerty" });
    books.Add(new Book { DisplayName = "Bill Wrestler" });
    books.Add(new Book { DisplayName = "Dr. Geri-Beth Hooper" });
    books.Add(new Book { DisplayName = "Dr. Keith Joyce-Purdy" });
    books.Add(new Book { DisplayName = "Sheri Spruce" });
    books.Add(new Book { DisplayName = "Burt Indybrick" });


    BookView.ItemsSource = books;
}

```

`ObservableCollection` is preferred generally in UI development over `List` due to the binding mechanism that is provided by this collection. Mainly, it implements `INotifyPropertyChanged`. You can find more about that in the Assembly Browser of the `ObservableCollection<>`:

```c#
namespace System.Collections.ObjectModel
{
    public class ObservableCollection<T> : Collection<T>, INotifyCollectionChanged, INotifyPropertyChanged
    {

    ...
    }
}
```  