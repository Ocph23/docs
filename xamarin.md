# Xamarin
- Remove Entry UnderLine
    *On Share Project
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

    * On Android Project
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