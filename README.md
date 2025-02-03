![image](https://github.com/user-attachments/assets/164b20b9-a8d4-48e9-9143-1ca88403ee61)# CoffeeMachine-main

using System;
using System.Collections.Generic;

namespace WaterShop
{
    internal class Ingredient
    {
        public string Name { get; set; }
        public int Quantity { get; private set; }

        public Ingredient(string name, int quantity)
        {
            Name = name;
            Quantity = quantity;
        }

        public bool UseIngredient(int amount)
        {
            if (Quantity >= amount)
            {
                Quantity -= amount;
                return true; 
            }
            return false; 
        }

        public void Refill(int amount)
        {
            Quantity += amount;
        }
    }

    internal class CoffeeMachine
    {
        public Dictionary<string, Ingredient> Ingredients { get; private set; }

        public CoffeeMachine()
        {
            Ingredients = new Dictionary<string, Ingredient>
            {
                { "Water", new Ingredient("Water", 200) },
                { "Coffee", new Ingredient("Coffee", 200) },
                { "Hot_Milk", new Ingredient("Hot Milk", 200) },
                { "Chocolate", new Ingredient("Chocolate", 200) }
            };
        }

        public bool MakeCoffee(string coffeeType)
        {
            Dictionary<string, int> recipe = GetRecipe(coffeeType);
            if (recipe == null) return false; 

            foreach (var item in recipe)
            {
                if (!Ingredients[item.Key].UseIngredient(item.Value))
                {
                    return false;
                }
            }
            return true;
        }

        private Dictionary<string, int> GetRecipe(string coffeeType)
        {
            if (coffeeType == "Late")
            {
                return new Dictionary<string, int>
        {
            { "Water", 300 },
            { "Coffee", 20 }
        };
            }
            else if (coffeeType == "Mocca")
            {
                return new Dictionary<string, int>
        {
            { "Water", 300 },
            { "Coffee", 20 },
            { "Chocolate", 10 }
        };
            }
            else if (coffeeType == "Americano")
            {
                return new Dictionary<string, int>
        {
            { "Water", 300 },
            { "Chocolate", 10 }
        };
            }
            else if (coffeeType == "Black")
            {
                return new Dictionary<string, int>
        {
            { "Water", 300 },
            { "Coffee", 20 }
        };
            }
            else
            {
                return null;  
            }
        }

    }
}


คำอธิบายแต่ละคลาส
1️⃣ Ingredient (วัตถุดิบ)
Attribute

Name : ชื่อของวัตถุดิบ เช่น น้ำ กาแฟ นม ช็อกโกแลต
Quantity : ปริมาณของวัตถุดิบที่มีอยู่
Method

UseIngredient(amount: int): หักปริมาณวัตถุดิบตามจำนวนที่ใช้ ถ้าพอใช้จะคืนค่า true ถ้าไม่พอจะคืนค่า false
Refill(amount: int): เติมวัตถุดิบเพิ่ม
2️⃣ CoffeeMachine (เครื่องชงกาแฟ)
Attribute

Ingredients: คลังเก็บวัตถุดิบที่มีอยู่ทั้งหมด (Dictionary ที่เก็บวัตถุดิบแต่ละประเภท เช่น น้ำ กาแฟ นม ช็อกโกแลต)
Method

MakeCoffee(coffeeType: string): รับประเภทกาแฟมาแล้วนำไปเช็ควัตถุดิบว่าพอไหม ถ้าพอก็ทำกาแฟและคืนค่า true ถ้าไม่พอคืนค่า false
GetRecipe(coffeeType: string): ส่งคืนสูตรของกาแฟแต่ละประเภทเป็น Dictionary เช่น {"Water": 300, "Coffee": 20}
Workflow ของ Coffee Machine
CoffeeMachine ถูกสร้างขึ้นมาและกำหนดวัตถุดิบให้มีค่าตั้งต้น (น้ำ 200, กาแฟ 200, นมร้อน 200, ช็อกโกแลต 200)
เมื่อเรียก MakeCoffee("Late")
ใช้ GetRecipe("Late") เพื่อดึงสูตรออกมา { "Water": 300, "Coffee": 20 }
เช็คว่า Ingredients["Water"] มีมากกว่า 300 ไหม (แต่มีแค่ 200) -> ไม่พอ
คืนค่า false แปลว่าทำกาแฟไม่ได้
ถ้ากาแฟสามารถทำได้ (วัตถุดิบพอ) ระบบจะลดปริมาณวัตถุดิบที่ใช้ไป
ปัญหาในโค้ด
วัตถุดิบตั้งต้นไม่พอสำหรับสูตรกาแฟบางประเภท

เช่น Water มีแค่ 200 แต่สูตร Late ต้องการ 300 ทำให้เครื่องทำกาแฟไม่สำเร็จตั้งแต่แรก
อาจต้องเพิ่มฟังก์ชันให้เติมวัตถุดิบอัตโนมัติถ้าไม่พอ
สูตรกาแฟอาจมีพิมพ์ผิด

Late ควรเป็น Latte
Mocca ควรเป็น Mocha
ควรใช้ Enum แทนการใช้ string เพื่อลดโอกาสพิมพ์ผิด
Americano ไม่มี Coffee

ในโค้ด Americano ใช้ Water กับ Chocolate ซึ่งผิดหลักการ อเมริกาโน่ควรใช้ Water กับ Coffee

