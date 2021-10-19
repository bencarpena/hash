//App Name  :   Hash
//Author    :   @bencarpena
//Version   :   3.1 ; 20210103

using System;

namespace hashconsole
{
    class Program
    {
        //public static string key = "Registry.GetValue("HKEY_LOCAL_MACHINE\Software\theedge", "TheEdge", Nothing)
        //public static int key_len = key.Length;

        static void Main(string[] args)
        {
            Console.WriteLine();
            Console.WriteLine("##############################");
            Console.WriteLine("Hash app v3.1 by @bencarpena");
            Console.WriteLine("##############################");
            Console.WriteLine();

            //####  Mode >>>
            Console.Write("Select mode (hash, unhash): ");
            string i_mode = Console.ReadLine();
            //Console.WriteLine("Mode selected is: " + i_mode);

            try
            {
                if (i_mode == "hash")
                {
                    Mode_Hash();
                    Console.Write("SUCCESS: " + DateTime.Now + " | Hash generated.");
                }
                else if (i_mode == "unhash")
                {
                    Mode_UnHash();
                    Console.Write("SUCCESS: " + DateTime.Now + " | Hash decoded.");
                }
                else
                {
                    Console.Write("ERROR: " + DateTime.Now + " | Not a valid input. Exiting...");
                }
            }

            catch (Exception e)
            {
                Console.WriteLine("ERROR: " + DateTime.Now + " | {0}", e.Message);
                Console.WriteLine();
            }

            finally
            {
                Environment.Exit(0);
            }
        }

        public static void Mode_Hash()
        {
            //####  String to Hash  >>>
            Console.Write("Enter string to Hash: ");
            string string_to_hash = Console.ReadLine();

            //####  Hash Code >>>
            Console.Write("Enter your Hash Key: ");

            var hashcode = string.Empty;
            ConsoleKey key;
            do
            {
                var keyInfo = Console.ReadKey(intercept: true);
                key = keyInfo.Key;

                if (key == ConsoleKey.Backspace && hashcode.Length > 0)
                {
                    Console.Write("\b \b");
                    hashcode = hashcode[0..^1];
                }
                else if (!char.IsControl(keyInfo.KeyChar))
                {
                    Console.Write("*");
                    hashcode += keyInfo.KeyChar;
                }
            } while (key != ConsoleKey.Enter);

            Console.WriteLine();
            //Console.WriteLine("Hash Code you entered is : " + hashcode);
            Console.WriteLine();
            Console.WriteLine("Hashed : " + BC_Encode(string_to_hash, hashcode));
            Console.WriteLine();

        }

        public static void Mode_UnHash()
        {
            //####  String to Hash  >>>
            Console.Write("Enter string to Decode: ");
            string string_to_decode = Console.ReadLine();

            //####  Hash Code >>>
            Console.Write("Enter your Hash Key: ");

            var hashcode = string.Empty;
            ConsoleKey key;
            do
            {
                var keyInfo = Console.ReadKey(intercept: true);
                key = keyInfo.Key;

                if (key == ConsoleKey.Backspace && hashcode.Length > 0)
                {
                    Console.Write("\b \b");
                    hashcode = hashcode[0..^1];
                }
                else if (!char.IsControl(keyInfo.KeyChar))
                {
                    Console.Write("*");
                    hashcode += keyInfo.KeyChar;
                }
            } while (key != ConsoleKey.Enter);

            Console.WriteLine();
            Console.WriteLine();
            Console.WriteLine("Unhashed : " + BC_Decode(string_to_decode, hashcode));
            Console.WriteLine();
        }

        public static string BC_Encode(string originalStr, string _hashcode)
        {
            int key_len = _hashcode.Length;
            Random rndnum = new Random();

            string encodedStr = originalStr;
            //int num = System.Convert.ToInt32(Conversion.Int((key_len * VBMath.Rnd()) + 1));
            int num = System.Convert.ToInt32(key_len * rndnum.NextDouble() + 1);

            if (num >= 10)
                num = 8;

            int i = 0;

            for (i = 1; i <= num; i++)
                encodedStr = Convert.ToBase64String(ConvertStringToByte(encodedStr), Base64FormattingOptions.None);
            char[] seed_array = _hashcode.ToCharArray();
            encodedStr = encodedStr + "+" + seed_array[num];
            encodedStr = Convert.ToBase64String(ConvertStringToByte(encodedStr), Base64FormattingOptions.None);
            return encodedStr;
        }


        public static byte[] ConvertStringToByte(string original)
        {
            System.Text.ASCIIEncoding ascii = new System.Text.ASCIIEncoding();
            return ascii.GetBytes(original.ToCharArray());
        }

        public static string ConvertByteToString(byte[] bytearr)
        {
            System.Text.ASCIIEncoding ascii = new System.Text.ASCIIEncoding();
            return new string(ascii.GetChars(bytearr));
        }


        public static string BC_Decode(string decodedStr, string _hashcode)
        {
            char[] seed_array = _hashcode.ToCharArray();
            string decoded = ConvertByteToString(Convert.FromBase64String(decodedStr));

            string letter;

            char[] separator = { '+' };
            //String[] strlist = decoded.Split(separator);


            letter = decoded.Split(separator)[1];
            decoded = decoded.Split(separator)[0];

            char char_letter = letter.ToCharArray()[0];

            int i = 0;
            for (i = 1; i <= seed_array.Length; i++)
            {
                if (seed_array[i] == char_letter)
                    break;
            }

            int j = 0;
            for (j = 1; j <= i; j++)
                decoded = ConvertByteToString(Convert.FromBase64String(decoded));
            return decoded;
        }

        /*
        public static string GenerateOTP()
        {
            string alphabets = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
            string small_alphabets = "abcdefghijklmnopqrstuvwxyz";
            string numbers = "1234567890";

            //string rbType = My.Computer.Registry.GetValue(@"HKEY_LOCAL_MACHINE\Software\sysencoder", "HKeyBC", null
            string rbType = "1";

            string characters = numbers;
            if (rbType == "1")
                characters += Convert.ToString(alphabets + small_alphabets) + numbers;

            int length = 6;
            string otp = string.Empty;
            for (int i = 0; i <= length - 1; i++)
            {
                string character = string.Empty;
                do
                {
                    int index = new Random().Next(0, characters.Length);
                    character = characters.ToCharArray()(index).ToString();
                }
                while (otp.IndexOf(character) != -1);
                otp += character;
            }

            return otp;
        }
        */

    }
}
