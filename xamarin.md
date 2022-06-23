# Xamarin
- Remove Entry UnderLine
    * On Share Project
  ```
    public class EntryCustom : Entry
    {
        public static readonly BindableProperty IsPasswordFlagProperty =
        BindableProperty.Create("IsPasswordFlag", typeof(bool), typeof(EntryCustom), defaultBindingMode: BindingMode.OneWay);
        public bool IsPasswordFlag
        {
            get { return (bool)GetValue(IsPasswordFlagProperty); }
            set { SetValue(IsPasswordFlagProperty, value); }
        }
    }

    ```

    * On Android Project
   ``` 
   [assembly: ExportRenderer(typeof(CustomEntry), typeof(CustomEntryRendererAndroid))]
    namespace RefreshViewDemo.Droid
    {
        public class CustomEntryRendererAndroid : EntryRenderer
        {
            protected override void OnElementChanged(ElementChangedEventArgs<Entry> e)
            {
                base.OnElementChanged(e);
                if (Control != null)
                {
                    GradientDrawable gd = new GradientDrawable();
                    gd.SetColor(Android.Graphics.Color.Transparent);
                    this.Control.SetBackground(gd);
                    this.Control.SetPadding(20, 0, 0, 0);
                    
                    CustomEntry customEntry= (CustomEntry) e.NewElement ;
                    if (customEntry.IsPasswordFlag)
                    {
                        this.Control.InputType = InputTypes.TextVariationVisiblePassword;
                    }
                
                }
            }
        public CustomEntryRendererAndroid(Context context) : base(context)
        {
        }
    }
    ```

- Embeded Font Icon
    * Download Icon https://fontawesome.com/download
        extract to ShareApp on Fonts folder
    
    * Open AssemblyInfo On ShareApp
        add
            ```
                            
                [assembly: ExportFont("Font Awesome 6 Brands-Regular-400.otf", Alias = "FABrand")]
                [assembly: ExportFont("Font Awesome 6 Free-Regular-400.otf", Alias = "FARegular")]
                [assembly: ExportFont("Font Awesome 6 Free-Solid-900.otf", Alias = "FASolid")] 

            ```

    * Example 
        On XAML : 
            ```
            <Label Text="&#xe1b0;" FontFamily="FASolid"></Label>
            ```

        On C# : 
            ```
                public const string Car = "\uf10b";
            ```
