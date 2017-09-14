# Android SegmentedControl + multi row support

![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/app/src/main/res/mipmap-hdpi/ic_launcher.png)

## Segmented control for android, with highly customizable properties 

![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/docs/intro.gif.gif)

## ScreenShots
![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/docs/device-2017-09-14-133621.png)
![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/docs/device-2017-09-14-133711.png)
![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/docs/device-2017-09-14-133736.png)
![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/docs/device-2017-09-14-133907.png)
![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/docs/device-2017-09-14-134003.png)
![N|Solid](https://raw.githubusercontent.com/RobertApikyan/SegmentedControl/release_v0.1/docs/device-2017-09-14-202249.png)





## Simple usage in XML
    <segmented_control.widget.custom.android.com.segmentedcontrol.SegmentedControl
                android:id="@+id/segmented_control"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_margin="8dp"
                app:columnCount="3"
                app:distributeEvenly="true"
                app:textVerticalPadding="6dp"
                app:radius="12dp"
                app:segments="@array/your_array_data" />

# Attributes 
 * <attr name="distributeEvenly" format="boolean" /> setDistributeEvenly(boolean)
 * <attr name="columnCount" format="integer" /> setColumnCount(int)
 * <attr name="segments" format="reference" /> addSegments(Object[]), addSegments(List)
 * <attr name="selectedStockColor" format="color" /> setSelectedStockColor(int)
 * <attr name="unSelectedStockColor" format="color" /> setUnSelectedStockColor(int)
 * <attr name="stockWidth" format="dimension" / setStockWidth(int)
 * <attr name="selectedBackgroundColor" format="color" /> setSelectedBackgroundColor(int)
 * <attr name="unSelectedBackgroundColor" format="color" /> setUnSelectedBackgroundColor(int)
 * <attr name="selectedTextColor" format="color"/> setSelectedTextColor(int)
 * <attr name="unSelectedTextColor" format="color"/> setUnSelectedTextColor(int)
 * <attr name="textSize" format="dimension"/> setTextSize(int)
 * <attr name="textHorizontalPadding" format="dimension"/> setTextHorizontalPadding(int)
 * <attr name="textVerticalPadding" format="dimension"/> setTextVerticalPadding(int)
 * <attr name="segmentVerticalMargin" format="dimension"/> setSegmentVerticalMargin(int)
 * <attr name="segmentHorizontalMargin" format="dimension"/> setSegmentHorizontalMargin(int)
 * <attr name="radius" format="dimension"/> setRadius(int)
 * <attr name="topLeftRadius" format="dimension"/> setTopLeftRadius(int)
 * <attr name="topRightRadius" format="dimension"/> setTopRightRadius(int)
 * <attr name="bottomRightRadius" format="dimension"/> setBottomRightRadius(int)
 * <attr name="bottomLeftRadius" format="dimension"/> setBottomLeftRadius(int)
 * <attr name="radiusForEverySegment" format="boolean"/> setRadiusForEverySegment(boolean)
        
### Note: After every configuration change call segmentedControl.notifyConfigIsChanged() method 
#### Example.
    ....
        segmentedControl.setStockWidth(width.intValue());
        segmentedControl.setColumnCount(columnCount);
        segmentedControl.notifyConfigIsChanged();
    ....

> SegmentedControl uses SegmentAdapter and SegmentViewHolder for displaying segments. There are allready exist a default implementations for SegmentAdapter (SegmentAdapterImpl) and SegmentViewHolder (SegmentViewHolderImpl), but if you want to make your custom implementation... well here is the steps 
### 1. define segment_item.xml inside layouts folder
    
    
    
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical">

        <TextView
            android:id="@+id/text_view"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:layout_gravity="center"
            android:layout_margin="2dp"
            android:background="@color/colorPrimary"
            android:gravity="center"
            android:textColor="@color/colorAccent"
            android:textSize="14sp" />
    </LinearLayout>

### 2. Craete SegmentViewHolder instance AppSegmentViewHolder (here I define the segment generic data type as a String)

    public class AppSegmentViewHolder extends SegmentViewHolder<String> {
        TextView textView;
    
        public AppSegmentViewHolder(@NonNull View sectionView) {
            super(sectionView);
            textView = (TextView) sectionView.findViewById(R.id.text_view);
        }

        @Override
        protected void onSegmentBind(String segmentData) {
            textView.setText(segmentData);
        }
    }
### 3. Create SegmentAdapter instance 
    public class AppSegmentAdapter extends SegmentAdapter<String, AppSegmentViewHolder> {

        @NonNull
        @Override
        protected AppSegmentViewHolder onCreateViewHolder(@NonNull LayoutInflater layoutInflater, ViewGroup viewGroup, int i) {
            return new AppSegmentViewHolder(layoutInflater.inflate(R.layout.item_segment, null));
        }
    }

### 4. Pass the adapter to the segmentedControl

    segmentedControl = (SegmentedControl) findViewById(R.id.segmented_control);
    segmentedControl.setAdapter(new AppSegmentAdapter());

### 5. Add segements data. 
    segmentedControl.addSegments(getResources().getStringArray(R.array.segments));
### Thatas it ) 
### Use SegmentedControlUtils helper class in order to define segment background type and background radius
