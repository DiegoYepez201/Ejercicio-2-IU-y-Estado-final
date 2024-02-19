# Ejercicio-2-IU-y-Estado-final
Ejercicio 2: IU y Estado

//Código android studio
package com.mexiti.costogasolina

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.annotation.DrawableRes
import androidx.annotation.StringRes
import androidx.compose.foundation.background
import androidx.compose.foundation.layout.Arrangement
import androidx.compose.foundation.layout.Column
import androidx.compose.foundation.layout.Row
import androidx.compose.foundation.layout.fillMaxSize
import androidx.compose.foundation.layout.fillMaxWidth
import androidx.compose.foundation.layout.height
import androidx.compose.foundation.layout.padding
import androidx.compose.foundation.layout.size
import androidx.compose.foundation.shape.RoundedCornerShape
import androidx.compose.foundation.text.KeyboardOptions
import androidx.compose.material3.Icon
import androidx.compose.material3.MaterialTheme
import androidx.compose.material3.Surface
import androidx.compose.material3.Switch
import androidx.compose.material3.Text
import androidx.compose.material3.TextField
import androidx.compose.runtime.Composable
import androidx.compose.runtime.getValue
import androidx.compose.runtime.mutableStateOf
import androidx.compose.runtime.remember
import androidx.compose.runtime.setValue
import androidx.compose.ui.Alignment
import androidx.compose.ui.Modifier
import androidx.compose.ui.graphics.Color
import androidx.compose.ui.res.painterResource
import androidx.compose.ui.res.stringResource
import androidx.compose.ui.text.font.FontWeight
import androidx.compose.ui.text.input.ImeAction
import androidx.compose.ui.text.input.KeyboardType
import androidx.compose.ui.text.style.TextAlign
import androidx.compose.ui.tooling.preview.Preview
import androidx.compose.ui.unit.dp
import androidx.compose.ui.unit.sp
import com.mexiti.costogasolina.ui.theme.CostoGasolinaTheme
import java.text.NumberFormat

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            CostoGasolinaTheme {
                // A surface container using the 'background' color from the theme
                Surface(
                    modifier = Modifier.fillMaxSize(),
                    color = MaterialTheme.colorScheme.background
                ) {
                    CostGasLayout()
                }
            }
        }
    }
}

@Composable
fun CostGasLayout() {
    var precioGasolina by remember {
        mutableStateOf("")
    }
    var cantLitros by remember {
        mutableStateOf("")
    }
    var propina by remember {
        mutableStateOf("")
    }
    var boolPropina by remember {
        mutableStateOf(false)
    }
    val precioLitro = precioGasolina.toDoubleOrNull() ?: 0.0
    val cantLitrosNum = cantLitros.toDoubleOrNull() ?: 0.0
    val propinaNum = propina.toDoubleOrNull() ?: 0.0
    val total = calcularTotal(precioLitro,cantLitrosNum, boolPropina = boolPropina, propinaNum = propinaNum)
    Column(
        modifier = Modifier.fillMaxSize()
            .padding(15.dp)
            .background(Color.LightGray,shape = RoundedCornerShape(15.dp)),
        verticalArrangement = Arrangement.Center
    ) {
        Text(
            text = stringResource(R.string.calcular_monto),
            modifier = Modifier.fillMaxWidth()
                .height(50.dp),
            fontWeight = FontWeight.Bold,
            fontSize = 25.sp,
            textAlign = TextAlign.Center
        )
        EditNumberField(
            label = R.string.ingresa_gasolina,
            leadingIcon = R.drawable.money_gas ,
            keyboardsOptions = KeyboardOptions.Default.copy(
                keyboardType = KeyboardType.Number,
                imeAction = ImeAction.Next
            ),
            value = precioGasolina,
            onValueChanged = {precioGasolina = it}
        )
        EditNumberField(
            label = R.string.ingresa_litros,
            leadingIcon = R.drawable.baseline_local_gas_station_24,
            keyboardsOptions = KeyboardOptions.Default.copy(
                keyboardType = KeyboardType.Text,
                imeAction = ImeAction.Next
            ) ,
            value = cantLitros,
            onValueChanged = {cantLitros = it}
        )
        EditNumberField(
            label = R.string.ingresa_propina,
            leadingIcon = R.drawable.moneyextra, //propine
            keyboardsOptions = KeyboardOptions.Default.copy(
                keyboardType = KeyboardType.Number,
                imeAction = ImeAction.Done
            ) ,
            value = propina,
            onValueChanged = {propina = it}
        )
        AddTip(boolPropina = boolPropina, onTipCheckedChange = {boolPropina = it})
        Text(
            text = stringResource(R.string.monto_total,total),
            fontWeight = FontWeight.Black,
            fontSize = 30.sp,
        )

    }
}
@Composable
fun EditNumberField(
    @StringRes label: Int,
    @DrawableRes leadingIcon: Int,
    keyboardsOptions:KeyboardOptions,
    value: String,
    onValueChanged: (String) -> Unit,
    modifier: Modifier = Modifier
){
    TextField(
        label = { Text(text = stringResource(id = label))  },
        value = value,
        singleLine = true,
        leadingIcon = { Icon(painter = painterResource(id = leadingIcon) , contentDescription = null) },
        keyboardOptions = keyboardsOptions,
        modifier = modifier.fillMaxWidth(),
        onValueChange = onValueChanged,
        )
}

@Composable
fun AddTip(
    boolPropina: Boolean,
    onTipCheckedChange: (Boolean)-> Unit,
    modifier: Modifier = Modifier
){
    Row(
        modifier = modifier
            .fillMaxWidth()
            .size(70.dp),
        verticalAlignment = Alignment.CenterVertically
    ) {
        Text(
            text = stringResource(id = R.string.ingresa_propina),
            modifier = Modifier.padding(20.dp)
        )
        Switch(
            checked = boolPropina ,
            onCheckedChange = onTipCheckedChange
        )
    }
}
private fun calcularTotal(precio:Double, cantLitros:Double, boolPropina: Boolean,propinaNum:Double): String {
    var total = precio * cantLitros
    if (boolPropina){
        total+=propinaNum
    }
    return NumberFormat.getCurrencyInstance().format(total)
}

@Preview(showBackground = true)
@Composable
fun CostGasLayoutPreview() {
    CostoGasolinaTheme {
        CostGasLayout()
    }
}
