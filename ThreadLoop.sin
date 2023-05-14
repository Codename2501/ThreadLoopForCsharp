using System;
using System.Threading;
using System.Threading.Tasks;

namespace ThreadLoop
{
    /* グローバル変数の定義 */
    class GlobalVariable
    {
        /* 正常系及び異常系の処理回数 */
        static public int errcount = 0;
        static public int erroutputcount = 0;
        static public int normalcount = 0;
        /* 正常系及び異常系の処理終了値 */
        public const int errterms = 100;
        public const int normalterms = 100;
        public bool globalerrflag = false;

    }

    class MainClass : GlobalVariable
    {
        public static void Main(string[] args)
        {
            Controler ct = new Controler();
            ct.CTMethod();
        }
    }
    class Controler : GlobalVariable
    {
        public void CTMethod()
        {
            try
            {
                MainRoutine mr = new MainRoutine();
                bool judge = mr.MTMethod();
                if (judge == false)
                {
                    /* MainLoopの戻り値がfalseの場合、異常系の処理に移行するため以下の処理を行う */
                    globalerrflag = true;
                    string str = null;
                    str.Split('\n');
                }
                /* 正常系の処理に移行する */
                Normal no = new Normal();
                Thread noThread = new Thread(new ThreadStart(no.NormalSwitch));
                if (noThread.IsAlive)
                {
                    noThread.Join();
                }
                noThread.Start();
            }
            catch (Exception e)
            {
                /* 異常系の処理に移行する */
                Err er = new Err();
                if (globalerrflag == false)
                {
                    globalerrflag = true;
                    er.ErrMethod(e);
                }
                Thread errThread = new Thread(new ThreadStart(er.ErrSwitch));
                if (errThread.IsAlive)
                {
                    errThread.Join();
                }
                errThread.Start();
            }
        }
    }
    class MainRoutine
    {
        public bool MTMethod()
        {
            string s = null;
            Random r = new Random();
            try
            {
                if (r.Next() % 2 == 0)
                {
                    s = "abc";
                }
                else if (r.Next() % 2 == 1)
                {
                    s = "null";
                }
                /* 変数sの中身次第で正常か異常かの判断をするs */
                s.Split('a');
            }
            catch (Exception e)
            {
                Err er = new Err();
                er.ErrMethod(e);
                return false;
            }
            return true;
        }
    }
    class Normal : GlobalVariable
    {
        public void NormalSwitch()
        {
            normalcount++;
            if (normalcount < normalterms)
            {
                Console.WriteLine("引き続き正常系の処理を実行します[回数[{0}]]", normalcount);
                Controler ct = new Controler();

                Thread controlerThread = new Thread(new ThreadStart(ct.CTMethod));
                if (controlerThread.IsAlive)
                {
                    controlerThread.Join();
                }
                controlerThread.Start();
            }
            else
            {
                Console.WriteLine("正常に処理を終了します");
            }
        }
    }
    class Err : GlobalVariable
    {
        public void ErrSwitch()
        {
            errcount++;
            if (errcount < errterms)
            {
                Console.WriteLine("_____________________________________________________________");
                Console.WriteLine("\n!!!引き続き異常系の処理から正常系への移行を実行します[回数[{0}]]!!!", errcount);
                Console.WriteLine("_____________________________________________________________");
                Controler ct = new Controler();
                Thread controlerThread = new Thread(new ThreadStart(ct.CTMethod));
                if (controlerThread.IsAlive)
                {
                    controlerThread.Join();
                }
                controlerThread.Start();
            }
            else
            {
                Console.WriteLine("___________________________________________________________");
                Console.WriteLine("\n!!!エラー処理回数が規定値以上になったため終了します[回数[{0}]]!!!", errcount);
                Console.WriteLine("___________________________________________________________");
            }
        }
        public void ErrMethod(object e)
        {
            erroutputcount++;
            Console.WriteLine("____________________________________");
            Console.WriteLine("エラーコードを出力します[総数[{0}]]\n{1}", erroutputcount, e);
            Console.WriteLine("____________________________________");
        }
    }
}
