# How to handle touch interaction using MR.Gesture in Xamarin.Forms ListView (SfListView)

You can load Xamrin.Forms [SfListView](https://help.syncfusion.com/xamarin/listview/overview?) inside [Mr.Gesture](https://www.mrgestures.com/)'s page and use Gesture [commands](https://www.mrgestures.com/#CommandsXAML) to handle touch.

You can refer the following article.

https://www.syncfusion.com/kb/11351/how-to-handle-touch-interaction-using-mr-gesture-in-xamarin-forms-listview-sflistview 

**Step 1:** Install the [Mr.Gesture](https://www.nuget.org/packages/MR.Gestures/) Nuget package in the shared code project.

**Step 2:** Create your Xaml page by inheriting Mr.Gesture.ContentPage.
``` c#
namespace ListViewXamarin
{
    public partial class MainPage : MR.Gestures.ContentPage
    {
        public MainPage()
        {
            InitializeComponent();
        }
    }
}
```
**Step 3:** Create [command](https://docs.microsoft.com/en-us/xamarin/xamarin-forms/app-fundamentals/data-binding/commanding) in the ViewModel class and handle the touch in the command execute method.
``` c#
namespace ListViewXamarin
{
    public class ContactsViewModel
    {
        public Command<object> MrGestureTapCommand { get; set; }
 
        public ContactsViewModel()
        {
            MrGestureTapCommand = new Command<object>(OnMrGestureCommandTapped);
        }
 
        private void OnMrGestureCommandTapped(object obj)
        {
            var tappedItem = obj as Contacts;
            App.Current.MainPage.DisplayAlert("Mr.Gesture tapped", tappedItem.ContactName + " tapped", "Ok");
        }
    }
}
```
**Step 4:** Add ListView to the content of the page. Add Mr.Gesture.Grid to the [ItemTemplate](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~ItemTemplate.html?) of SfListView and bind ViewModel command to TappedCommand of Mr.Gesture.Grid.

``` xml
<mr:ContentPage xmlns:mr="clr-namespace:MR.Gestures;assembly=MR.Gestures"
             xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:ListViewXamarin"
             xmlns:syncfusion="clr-namespace:Syncfusion.ListView.XForms;assembly=Syncfusion.SfListView.XForms"
             x:Class="ListViewXamarin.MainPage">
    <mr:ContentPage.BindingContext>
        <local:ContactsViewModel/>
    </mr:ContentPage.BindingContext>
 
    <mr:ContentPage.Content>
        <StackLayout>
            <syncfusion:SfListView x:Name="listView"
                                   ItemSpacing="1" 
                                   ItemSize="60"
                                   ItemsSource="{Binding ContactsInfo}">
                <syncfusion:SfListView.ItemTemplate >
                    <DataTemplate>
                        <mr:Grid TappedCommand="{Binding Path=BindingContext.MrGestureTapCommand, Source={x:Reference listView}}" TappedCommandParameter="{Binding .}">
                                <mr:Grid.ColumnDefinitions>
                                    <ColumnDefinition Width="70" />
                                    <ColumnDefinition Width="*" />
                                </mr:Grid.ColumnDefinitions>
                                <Image Source="{Binding ContactImage}" VerticalOptions="Center" HorizontalOptions="Center" HeightRequest="50" WidthRequest="50"/>
                                <Grid Grid.Column="1" RowSpacing="1" Padding="10,0,0,0" VerticalOptions="Center">
                                    <Label LineBreakMode="NoWrap" TextColor="#474747" Text="{Binding ContactName}"/>
                                    <Label Grid.Row="1" Grid.Column="0" TextColor="#474747" LineBreakMode="NoWrap" Text="{Binding ContactNumber}"/>
                                </Grid>
                        </mr:Grid>
                    </DataTemplate>
                </syncfusion:SfListView.ItemTemplate>
            </syncfusion:SfListView>
        </StackLayout>
    </mr:ContentPage.Content>
</mr:ContentPage>
```
