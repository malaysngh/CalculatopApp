# CalculatopApp
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import android.widget.Button
import android.widget.TextView
import android.widget.Toast
class MainActivity : AppCompatActivity() {
    private var tv1 : TextView? = null
    var lastNumeric : Boolean=false
    var lastDot : Boolean=true
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        tv1 = findViewById(R.id.tvInput )
    }
    fun onDigit(view: View){
//        Toast.makeText(this,"Button Clicked",Toast.LENGTH_SHORT).show()
        tv1?.append((view as Button).text)
        lastNumeric=true
        lastDot= false}
    fun onClear(view :View){
        tv1?.text=""
    }
    fun onDecimalPoint(view:View){
        if(lastNumeric && !lastDot){
            tv1?.append(".")
            lastNumeric=false
            lastDot=true
        }
    }
    fun onOperator(view:View){
        tv1?.text?.let {
            if(lastNumeric && !isOperatorAdded(it.toString())){
                tv1?.append((view as Button).text)
                lastNumeric= false
                lastDot=false
        }
        }
    }
     fun onEqual(view: View){
        if(lastNumeric){
            var tvValue=tv1?.text.toString()
            var prefix=""
            try{
                if (tvValue.startsWith("-")){
                    prefix="-"
                        tvValue= tvValue.substring(1)
                    }
                if (tvValue.contains("-")){
                    val splitValue = tvValue.split("-")
                    var one = splitValue[0]
                    var two=splitValue[1]
                    tv1?.text=removeZeroAfterDot(( one.toDouble()-two.toDouble()).toString())
                    if( prefix.isNotEmpty()){
                        one = prefix + one
                    }
                }
                if (tvValue.contains("+")){
                    val splitValue = tvValue.split("+")
                    var one = splitValue[0]
                    var two=splitValue[1]
                    tv1?.text=removeZeroAfterDot(( one.toDouble()+two.toDouble()).toString())
                    if( prefix.isNotEmpty()){
                        one = prefix + one
                    }
                }
                if (tvValue.contains("/")){
                    val splitValue = tvValue.split("/")
                    var one = splitValue[0]
                    var two=splitValue[1]
                    tv1?.text=removeZeroAfterDot(( one.toDouble()/two.toDouble()).toString())
                    if( prefix.isNotEmpty()){
                        one = prefix + one
                    }
                }
                if (tvValue.contains("*")){
                    val splitValue = tvValue.split("*")
                    var one = splitValue[0]
                    var two=splitValue[1]
                    tv1?.text=removeZeroAfterDot(( one.toDouble()*two.toDouble()).toString())
                    if( prefix.isNotEmpty()){
                        one = prefix + one
                    }
                }
            }
            catch (e:ArithmeticException){
                e.printStackTrace()
            }
        }
    }
    private fun isOperatorAdded(value : String):Boolean {
        return if (value.startsWith("-")){
            false
        }else{
            value.contains("/")||
                    value.contains("+")||
                    value.contains("*")||
                    value.contains("-")
        }
    }
    private fun removeZeroAfterDot(result: String) : String{
         var value = result
        if(result.contains(".0")){
            value=result.substring(0,result.length-2)

        }
        return value
    }
}
