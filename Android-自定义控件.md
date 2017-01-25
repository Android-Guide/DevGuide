# 自定义控件

## 实现一个可根据内容调整字体大小的 TextView

Android 系统的 TextView 并不支持这个功能，所以需要自己实现。

`AutoAjustSizeTextView.java` :

```
package net.saick.android.control;

import net.saick.android.calcapp.R;
import android.content.Context;
import android.content.res.TypedArray;
import android.graphics.Paint;
import android.util.AttributeSet;
import android.util.TypedValue;
import android.widget.TextView;

public class AutoAjustSizeTextView extends TextView {

	private static float DEFAULT_MIN_TEXT_SIZE = 10;  
    private static float DEFAULT_MAX_TEXT_SIZE = 16;

    // Attributes  
    private Paint mPaint;  
    private float minTextSize, maxTextSize;

	public AutoAjustSizeTextView(Context context) {
		super(context);

		initialise();
	}

	private void initialise() {  
        mPaint = new Paint();  
        mPaint.set(this.getPaint());  

        maxTextSize = this.getTextSize();  

        if (maxTextSize <= DEFAULT_MIN_TEXT_SIZE) {  
            maxTextSize = DEFAULT_MAX_TEXT_SIZE;  
        } else {
        	// continue
        }

        minTextSize = DEFAULT_MIN_TEXT_SIZE;  
    }

	public AutoAjustSizeTextView(Context context, AttributeSet attrs) {
		super(context, attrs);

		initialise();

		TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.AutoAjustSizeTextView);  
		minTextSize = ta.getFloat(R.styleable.AutoAjustSizeTextView_minTextSize, DEFAULT_MIN_TEXT_SIZE);
		ta.recycle();
	}

	public AutoAjustSizeTextView(Context context, AttributeSet attrs, int defStyle) {
		super(context, attrs, defStyle);

		initialise();

		TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.AutoAjustSizeTextView);  
		minTextSize = ta.getFloat(R.styleable.AutoAjustSizeTextView_minTextSize, DEFAULT_MIN_TEXT_SIZE);
		ta.recycle();
	}

	/**
     * Re size the font so the specified text fits in the text box * assuming
     * the text box is the specified width.
     */  
    private void refitText(String text, int textWidth) {  
        if (textWidth > 0) {  
            int availableWidth = textWidth - this.getPaddingLeft() - this.getPaddingRight();  
            float trySize = maxTextSize;  
            mPaint.setTextSize(trySize);

            while ((trySize > minTextSize) && (mPaint.measureText(text) > availableWidth)) {  
                trySize -= 1;  
                if (trySize <= minTextSize) {  
                    trySize = minTextSize;  
                    break;  
                }  
                mPaint.setTextSize(trySize);  
            }  
            //this.setTextSize(trySize);
            this.setTextSize(TypedValue.COMPLEX_UNIT_PX, trySize);
        }
    };

    @Override  
    protected void onTextChanged(CharSequence text, int start, int before, int after) {  
        super.onTextChanged(text, start, before, after);  
        refitText(text.toString(), this.getWidth());  
    }  

    @Override  
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {  
        if (w != oldw) {  
            refitText(this.getText().toString(), w);  
        }  
    }
}

```

`res/values/attrs.xml` :

```
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
    <declare-styleable name="AutoAjustSizeTextView">
        <attr name="minTextSize" format="float"></attr>  
    </declare-styleable>  
</resources>
```

使用示例：

```
...

<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:AutoAjustSizeTextView="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical" >
	<LinearLayout
	    android:layout_width="match_parent"
	    android:layout_height="0dp"
	    android:layout_weight="2"
	    android:orientation="vertical"
	    >

	    <net.saick.android.calcapp.control.CalcTextView
	        android:id="@+id/tv_jisuan"
	        android:layout_width="match_parent"
	        android:layout_height="match_parent"
	        android:gravity="bottom|right"
	        android:singleLine="true"
	        AutoAjustSizeTextView:minTextSize="150"
	        android:text="@string/default_value"
	        android:textSize="90sp" />

            ...
```

### refs

[自动调整TextView字体大小以适应文字长度](http://shirlly.iteye.com/blog/1481880)  
[Android自定义控件并且使其可以在xml中自定义属性](http://blog.csdn.net/mthhk008/article/details/30070119)
